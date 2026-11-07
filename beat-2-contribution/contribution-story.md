# Contribution: The console's new-item Description box sits one column off

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Melody Wong
**GitHub Username:** melmel812
**Issue:** https://github.com/itsOmidKarami/kraft/issues/1309
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the issue-scout with a Python bug-fix search strategy and ran a live scan**

Installed the issue-scout skill over the unit-1 sandbox, wrote a fit profile targeting Python bug fixes, and authored two `gh search issues` queries with ranking filters (drop assignees, drop stale, fewest comments first). The scan surfaced 198 candidates, ranked them to 5, and graded all 5 against the rubric; one was rejected for having a bot-generated placeholder body with no defined scope, which the bounded-scope check caught. The rubric held up on all five without needing a patch, so no changes were made to rubric.md; the auto-generated issue was a clear failure signal, not a gray area.

**Unit 6: Installed the voice guard, patched the rubric scope check, re-graded picks, and wrote a repo memory file for kraft**

The voice guard (`check_voice.py` as a PreToolUse hook) now intercepts every `gh issue comment` / `gh pr create` call before it posts. The rubric patch replaced the strict "Bounded scope" check with the more permissive "scope-feasible" row — the old check rejected most real-world issues with any open edge; the new one only rejects issues with no single identifiable piece of work. Re-grading with the patched rubric revealed that two of my three previous accepts (mcp-memory-service #1893 and discord-youtube-tracker #4) had open PRs fixing them since last week's scan; only kraft #1309 remained. The repo memory file (CLAUDE.local.md) distills the CONTRIBUTING.md rules, the subsystem my issue touches, and the exact test command into the clone root so Claude doesn't re-orient each session.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I built an issue-scout with two `gh search issues` queries targeting Python bug fixes labeled "good first issue." The scout searched broadly (100 results per query), dropped any issue with an assignee or stale activity, and sorted survivors by fewest comments. From 198 unique results it graded the top 5 against my rubric; four accepted, one rejected for having a bot-generated placeholder body with no defined scope.

This issue ranked first among the accepted candidates. The fix is a single CSS padding rule for one widget in one screen — the scope couldn't be tighter. The issue body names the exact file, line range, fix, and test command, so there's no design decision left for me to make. Kraft released v2.5.0 five days before the scan and had commits the same day, so the repo is clearly active. My scout accepted it on all four required checks, and the newcomer signal (preferred) check passed too. I expect to learn how Textual's widget styling and headless pilot testing work. My claim comment: https://github.com/itsOmidKarami/kraft/issues/1309#issuecomment-6704022809

**Toolkit**

- Tool: issue-scout (two gh search queries: good-first-issue + bug + python; bug + python), ranked by fewest comments after dropping assignees and stale results; re-graded in unit 6 with the scope-feasible rubric patch
- What it produced: 4 accepted candidates out of 5 graded in unit 5; re-grade in unit 6 showed only kraft #1309 still accepts — mcp-memory-service #1893 and discord-youtube-tracker #4 both had open PRs fixing them (not-claimed fail); difficulty note for #1309: straightforward
- Difficulties: the strict "Bounded scope" check was replaced with "scope-feasible" in unit 6 because it rejected most real issues with any open edge; the re-grade also caught that two previously accepted issues had been claimed by open PRs since the first scan
- What changed: rubric.md scope check replaced with scope-feasible row; difficulty note section added; scan-notes.md updated with the patch rationale

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS 15.5 (arm64, Apple M-series). Forked itsOmidKarami/kraft to melmel812/kraft and cloned locally. Installed `just` via Homebrew and `uv` via pip, then ran `just setup` (uv sync + npm ci). No challenges: setup completed cleanly on first run.

- kraft v2.5.0 (commit 2026-11-06, main branch)
- Python 3.15.3 (via uv, kraft's `.venv`)
- Textual 2.5.6 (pinned in kraft's `uv.lock`)

**Steps to Reproduce**

1. Clone the fork and run `just setup`.
2. Run the headless pilot script below (no interactive terminal needed):

```python
import asyncio
from textual.app import App, ComposeResult
from textual.containers import Horizontal
from textual.widgets import Input, TextArea

class Demo(App):
    CSS = """
    .row { height: 1; }
    .row > Input, .row > TextArea { width: 1fr; }
    """
    def compose(self) -> ComposeResult:
        with Horizontal(classes='row'):
            yield Input(compact=True, id='inp')
        with Horizontal(classes='row'):
            yield TextArea(compact=True, id='ta')

async def main():
    app = Demo()
    async with app.run_test(size=(80, 10)) as pilot:
        inp = app.query_one('#inp', Input)
        ta = app.query_one('#ta', TextArea)
        print(f'Input  offset.x={inp.content_offset.x}  padding={inp.styles.padding}')
        print(f'TextArea offset.x={ta.content_offset.x}  padding={ta.styles.padding}')

asyncio.run(main())
```

3. Observed: `TextArea offset.x=1` vs `Input offset.x=0` — the TextArea content starts one column to the right.

**Reproduction Evidence**

- **Commit showing reproduction:** none — repro is a standalone script run against unmodified main branch
- **Output:**
  ```
  Input  offset.x=0  padding=Spacing(top=0, right=0, bottom=0, left=0)
  TextArea offset.x=1  padding=Spacing(top=0, right=1, bottom=0, left=1)
  ```
- **My findings:** Textual's `Input` DEFAULT_CSS sets `padding: 0` inside its `&.-textual-compact` block; `TextArea`'s compact variant only removes the border — `padding: 0 1` is never cleared. The kraft CSS rule at `kraft.tcss:508` sets `width: 1fr` for both widgets but adds no padding override for TextArea. This matches the issue's diagnosis exactly. 22 existing tests in `tests/console/test_new_item.py` pass on main; none assert the Description widget's left-edge alignment.

**Toolkit**

- Tool: repro-check (live mode, full package grade against kraft/issues/1309)
- What it produced: verdict accept on all 7 required checks; headless Textual pilot output showing `TextArea offset.x=1` vs `Input offset.x=0` as the concrete artifact
- Difficulties: `just` and `uv` were not on PATH initially; installed both before setup could run
- What changed: nothing in the rubric or repro process; the pilot script approach worked on first attempt

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
