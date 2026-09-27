# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

melmel812

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5853383740

I'd like to investigate this as a first contribution. The bug is clear from reading the source: `api/routes/health.py` builds the Redis client with `settings.redis_host` and `settings.redis_port`, but `core/config.py`'s `Settings` class only defines `redis_url` — those two attributes don't exist, so every call raises an `AttributeError` that the broad `except` silently swallows, always marking Redis unhealthy. My next step is to reproduce the 503 locally and then update the health check to use `redis.from_url(settings.redis_url)` instead.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/62#issuecomment-5853469045

Environment: Python 3.11.9, macOS 15.6.1 (arm64), pathreview commit f89c06f (fork of codepath/pathreview-ai301-fa26-s1).

The bug is in `api/routes/health.py`: it builds the Redis client with `settings.redis_host` and `settings.redis_port`, but `core/config.py`'s `Settings` class only defines `redis_url`. Accessing either missing attribute raises an `AttributeError` that the broad `except Exception` block silently catches, logging `redis_health_check_failed` and marking Redis unhealthy regardless of its actual state.

Steps (no server required — the attribute access fails before any network call):

```
$ cd pathreview-ai301-fa26-s1
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
```

Output:

```
redis_health_check_failed error= AttributeError("'Settings' object has no attribute 'redis_host'")
Traceback (most recent call last):
  File "<string>", line 4, in <module>
  File ".../pydantic/main.py", line 1042, in __getattr__
    raise AttributeError(f'{type(self).__name__!r} object has no attribute {item!r}')
AttributeError: 'Settings' object has no attribute 'redis_host'
```

Control: `settings.redis_url` is defined and returns `'redis://localhost:6379/0'`, confirming the field exists under a different name.

Expected: the health check uses `settings.redis_url` to connect to Redis and reports its actual status.

Actual: `settings.redis_host` raises `AttributeError` on the first line of the Redis block; the except swallows it and Redis is always reported unhealthy.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full): 18/20 — below the bar; category floor unmet: no match in disclosure. Two disagreements: pkg-12 (graded reject, gold accept) and pkg-20 (graded accept, gold reject).

Run 2 (--only pkg-12,pkg-20,pkg-03): 3/3 — all agreed after rubric revisions. Used to confirm fixes before the confirming full run.

Run 3 (full confirming): 20/20 — PASS. All five categories matched (clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4).

**Package analysis**

pkg-20 (disclosure category, gold: reject). My rubric initially graded it accept. pkg-20 is a ghostty issue with an excellent repro on every proof check — environment recorded, steps followable, behavior matches the issue, outcome honest. My first `disclosure-if-required` check read: "if the policy requires disclosure only when AI is used and the comments are written in human voice with no apparent AI assistance, that satisfies the policy." The grader read the comments as human-voiced and passed the check, missing that ghostty's policy is blanket — "All AI usage in any form must be disclosed" — not a conditional-on-apparent-AI-use requirement. I revised the check to distinguish a blanket policy from a quality-standard policy. After the revision, pkg-20 correctly rejected on disclosure.

**Check rationale**

Quoted from `tools/repro-check/rubric.md`:

> | disclosure-if-required | The repo-facts block's contribution policy field and the text of the claim and repro comments | If the repo's stated policy is a blanket AI-disclosure requirement (e.g., "all AI usage must be disclosed" or "AI-assisted issues must disclose the tool and extent"), the comments must include an explicit disclosure; in eval mode, treat all packages as AI-assisted for this check, so absence of disclosure under a blanket policy fails. If the policy requires only that comments be written by humans in their own words (a quality standard, not a disclosure requirement), a human-voiced comment satisfies it without a disclosure statement. If there is no stated AI policy, this check passes automatically | required |

The check reads this way because the first version collapsed two distinct policy types into one rule. Ghostty's "all AI usage must be disclosed" and ripgrep's "comments must be written by humans in their own words" look similar but require different evidence: ghostty needs an explicit disclosure statement regardless of how the comment reads; ripgrep needs the comment to be human-authored, which the comment's own voice demonstrates. Splitting them — blanket-policy vs. quality-standard-policy — was what let the check correctly accept pkg-03 (ripgrep, human-voiced) and reject pkg-20 (ghostty, no disclosure).

**Trade-offs**

The `steps-followable` check was also revised: the original version failed any step that referenced an external resource without checking whether that resource was the public issue itself. pkg-12 (prettier, gold: accept) was rejected because its repro report said "ran the issue's script verbatim" — the grader read this as pointing to something inaccessible. After adding the clause "references to the issue's own reproduction script or example are followable when the issue context in the package provides that information," pkg-12 correctly accepted. To confirm the loosening did not flip pkg-06 or pkg-18 (both unfollowable-comms rejects), I ran --only pkg-06,pkg-18 as canaries before the confirming full run. Both still rejected: pkg-06 fails environment-recorded (no OS, driver, or minikube version), and pkg-18 fails steps-followable on a different ground (private monorepo, not an issue-context reference). The canary confirmed the change was targeted.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
