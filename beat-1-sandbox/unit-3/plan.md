# Plan: Fix Redis health check using undefined settings attributes (issue #62)

## Diagnosis

The Redis health check in `api/routes/health.py` builds a Redis client with
`settings.redis_host` and `settings.redis_port`, but `core/config.py`'s `Settings` class
defines neither attribute — only `redis_url`. Accessing either missing attribute raises an
`AttributeError` at the first line of the Redis block. The broad `except Exception` catch
swallows it, logs `redis_health_check_failed`, and marks Redis unhealthy regardless of its
actual state.

Grounded in the repro: the control run shows `settings.redis_url` returns
`'redis://localhost:6379/0'`, confirming the correct attribute exists. The repro shows the
error is raised by `pydantic_settings`' `__getattr__` on `settings.redis_host` — before any
network call is made — so the failure is not connection-dependent.

## Scope

In scope: the Redis client construction in `api/routes/health.py`, changed to use
`redis.from_url(settings.redis_url)` so it uses the attribute that actually exists.
Not in scope: any change to `core/config.py` (the `redis_url` field is correct and needs no
addition), the PostgreSQL or vector DB health check paths, error message wording, or the
broad except catch pattern (a separate concern).

## Files

- `api/routes/health.py` — replace the `redis.Redis(host=..., port=...)` call with
  `redis.from_url(settings.redis_url, decode_responses=True)`

## Approach

Replace lines:
```python
r = redis.Redis(
    host=settings.redis_host,
    port=settings.redis_port,
    db=0,
    decode_responses=True,
)
```
with:
```python
r = redis.from_url(settings.redis_url, decode_responses=True)
```

`redis.from_url` parses the URL (scheme, host, port, db) and returns a Redis client; no
separate host/port attributes needed. The `redis_url` default already encodes the db number
(`/0`), so the explicit `db=0` argument is redundant and is dropped.

## Test plan

Re-run the repro script against the changed code:

**Before (from repro evidence):**
```
$ python3 -c "
from core.config import settings
import redis, traceback
try:
    r = redis.Redis(host=settings.redis_host, port=settings.redis_port, db=0, decode_responses=True)
    r.ping()
except Exception as exc:
    print('redis_health_check_failed error=', repr(exc))
    traceback.print_exc()
"
redis_health_check_failed error= AttributeError("'Settings' object has no attribute 'redis_host'")
```

**Expected after fix:** the `AttributeError` is gone; the script raises a
`redis.exceptions.ConnectionError` (no Redis server is running) instead of an
`AttributeError`. This confirms the attribute lookup is fixed and the code reaches the actual
network call.

Verification command after the change:
```bash
python3 -c "
from core.config import settings
import redis
r = redis.from_url(settings.redis_url, decode_responses=True)
print('client created:', r)
try:
    r.ping()
    print('ping: ok')
except redis.exceptions.ConnectionError as exc:
    print('ConnectionError (expected, no server):', exc)
except AttributeError as exc:
    print('AttributeError (still broken):', exc)
"
```
A `ConnectionError` or `ping: ok` outcome confirms the fix. An `AttributeError` means the fix did not apply.

## Risks and unknowns

- `redis.from_url` respects the full URL including the database index encoded as the path
  component (`/0`); verified against `redis-py` docs that `db` parameter in `from_url` is
  overridden by the URL path. No separate `db=0` needed.
- If the `redis_url` setting is overridden by an environment variable to include auth
  credentials (`redis://:password@host:port/db`), `from_url` handles that correctly — no
  change needed.
- No existing tests cover the health check Redis path; adding a unit test is not in scope
  for this fix (a separate follow-up issue could add integration-style health check tests).

## Deviations

Nothing changed; the plan held. The test plan said "a ConnectionError is the expected pass outcome" when no server is running, but Redis was actually running locally, so `ping: ok` was observed instead. Both outcomes confirm the AttributeError is gone and the fix is correct — the plan's goal was to verify no AttributeError, which was met.
