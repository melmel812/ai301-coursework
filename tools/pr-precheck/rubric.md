# Rubric: is this pull request ready to submit?

<!--
Failure families this rubric covers, matching the harness scoring
categories:
  silent-drift     — diff does more or less than the plan, or the
                     description claims fidelity the diff contradicts
  not-tested       — no observable before/after, evidence on the wrong
                     path, or repo's own checks never shown
  unreviewable     — debug prints, dead code, commented-out blocks, or
                     unrelated hunks bury the change
  standards-wall   — required template sections ignored, or the repo's
                     stated AI-disclosure policy unmet
A PR that fails none of them is a clear accept.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks read against the plan's scope statement, named files, and deviation notes (in eval: the plan-context block's scope pair and `## Deviations` note if present; in live: `plan.md` including any `## Deviations` section). Then the description's claims about what changed, read against the actual diff. | Every changed file and hunk is either (a) named or entailed by the plan's stated scope, or (b) explicitly noted in a deviation appended to the plan. AND the description does not claim the diff contains something it does not, nor deny something it does (either direction is silent drift). A diff that delivers less than the plan with an honest deviation note in plan.md and in the description still passes; a diff that delivers less with a description claiming completeness fails. A deviation recorded only in the diff (not in plan.md) still fails. | required |
| test-evidence | The test-evidence section's before/after output, read against the plan's test plan and the repro's specific failure command and observable artifact. The outcome of the repo's own checks (test suite, lint, CI commands) if the repo's template or contribution guide states they are required. | At least one observable before/after demonstrates that the specific failure the repro showed changed in the expected direction — the same scenario, not a control that was never broken. If the repo's template or contribution guide requires named checks (e.g., `cargo test`, `make lint`, a named CI suite), their outcome must be shown and passing (or honestly explained if a failure is unrelated). Fails if: the before/after is absent; only "tests pass" is stated with no named outcome for the fix; the evidence exercises a control path that was already passing; required repo checks are not shown. | required |
| diff-quality | The unified diff and commit list, read for debris: debug print statements (eprintln!, println!, console.log, print, etc.), commented-out code blocks (not doc comments), dead functions or dead imports added by this PR, formatting-only or whitespace-only hunks unrelated to the fix, commit messages suggesting the diff was not cleaned up (wip, fix, fmt, misc). | The diff contains only the fix and any directly required supporting code (tests, changelog entry if required). No debug statements, no commented-out experiments, no dead functions, no unrelated formatting hunks, no commit messages indicating work-in-progress state. A single meaningful commit message is not required, but commits named "wip", "fix", or "misc cleanup" are a signal that the diff was not cleaned up. | required |
| standards-comms | The repo-facts block's PR template asks and stated AI-use policy; the description's template sections read against what the template requires; thread highlights for maintainer direction. | Every section the PR template explicitly requires has real content (not a placeholder, not left blank). If the repo's contribution policy has a blanket AI-disclosure requirement (any phrase like "all AI usage must be disclosed" or "AI-assisted contributions must disclose"), an explicit disclosure naming the tool and the extent of assistance appears in the description (in eval mode, treat every package as AI-assisted). A human-voice policy ("comments must be in your own words") is satisfied when the text reads as human-authored; it does not require a disclosure statement. No template, no AI policy, and no maintainer direction: check passes. If a maintainer direction is present in the thread highlights (a named culprit, a stated fix approach, a testing request), the description or plan must engage it — acknowledge, build on, or depart from it with a stated reason; silence is not engagement. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check
fails. `unclear` counts as fail: a PR whose evidence cannot be verified
from the package is not ready to submit. Preferred checks never change
the verdict.
