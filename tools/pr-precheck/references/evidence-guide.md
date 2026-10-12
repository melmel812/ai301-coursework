# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives — in an eval bundle:** The plan's scope statement is
in the plan-context block, under the in-scope / not-in-scope line. The
plan's named files are listed there too. Any deviation notes appear
under a `## Deviations` heading at the end of the plan-context block.
The diff is in the candidate PR's diff section. The description's
claims about what the diff contains are in the Description section.

**Where it lives — in live mode:** The plan's scope is in `plan.md`,
Scope section (or the equivalent paragraph). Deviation notes are in
`plan.md` under `## Deviations`, appended during the build. The branch
diff is produced by `git diff main...HEAD` (three dots), run from the
working copy's root — this shows every committed change the branch
makes relative to main. The description's claims are in `pr_draft.md`.

**What good looks like:** Every changed file either (a) is named in the
plan's scope or directly required to implement it (a test file, a
changelog entry the plan mentioned), or (b) has a deviation note in
`plan.md` AND the description restates the deviation. The description
neither overclaims ("implements the plan in full" when a deliverable is
missing) nor ignores extra work in the diff. A terse PR that delivers
only what the plan asked passes; a PR that bundles adjacent work the
plan scoped out fails even if that work is reasonable.

**Key traps:**
- A diff that adds a new config option, a refactored helper, or
  documentation the plan never mentioned is silent-drift-more, even
  when the description calls it a minor touch.
- A description that claims "the docs now document X" when the diff
  has no docs changes is silent-drift-less: the description overclaims.
- A deviation recorded only in the diff (not in `plan.md` and not in
  the description) is silent drift, not an honest deviation.

## Test evidence (harness category: not-tested)

**Where it lives — in an eval bundle:** The test-evidence section of
the candidate PR. The plan's test plan is in the plan-context block.
The repro scenario (the specific failing command and observable
artifact) is in the plan-context's repro-evidence excerpt.

**Where it lives — in live mode:** `test_evidence.md`, containing the
output of the student's checks. The plan's test plan is in `plan.md`,
Test plan section. The repro scenario is the student's unit-2 repro
comment or the house repro pack.

**What good looks like:** The test evidence re-runs the specific
scenario the repro demonstrated as failing. The before (on main) and
after (on the branch) are both shown, and the observable outcome
matches the plan's stated expected-after. If the repo's template or
contribution guide names required checks (e.g., `cargo test`, `make
lint`, a CI suite), their output is shown and passing — or the
description explains any failing check as unrelated and not introduced
by this PR.

**Key traps:**
- Evidence that exercises a control path (the scenario that was already
  passing) proves nothing about the fix. The before/after must cover
  the same scenario the repro showed failing.
- "Verified locally" or "tested manually" with no artifact is not
  evidence.
- "Tests pass" with no named outcome for the fix is not evidence.
- A required repo check (one the template or contribution guide
  explicitly names) that is not shown counts as "not run."

## Diff quality (harness category: unreviewable)

**Where it lives — in an eval bundle:** The candidate PR's diff section
and commits list.

**Where it lives — in live mode:** The output of `git diff main...HEAD`
and `git log main..HEAD --oneline`.

**What good looks like:** The diff shows exactly the fix and its
directly required supporting code (tests, a changelog entry). Every
hunk is either the fix itself, a test for it, or an entry an explicit
repo requirement asked for. Commit messages describe what changed, not
the workflow state.

**Debris tells:**
- Debug print statements: `eprintln!`, `println!`, `console.log`,
  `print()`, `dbg!`, `dump()` in production code (not tests).
- Commented-out code blocks: `// let mut ...`, `/* first attempt */`,
  blocks left in with `// keeping for reference` notes.
- Dead functions: functions added by this PR with no call sites, or
  marked `#[allow(dead_code)]`.
- Unrelated formatting hunks: hunks that only change whitespace,
  re-indent, or reorder imports in files the fix didn't need to touch.
- Work-in-progress commit messages: "wip", "fix", "fmt + cleanup",
  "misc" as the entire message — signals that the diff was not cleaned
  up before submission.

## Standards and comms (harness category: standards-wall)

**Where it lives — in an eval bundle:** The repo-facts block's PR
template description and contribution policy field. The description's
sections in the candidate PR. The thread highlights for maintainer
direction.

**Where it lives — in live mode:** The repo's `.github/PULL_REQUEST_TEMPLATE.md`
(fetched via `gh` or the web). The repo's `CONTRIBUTING.md` or
`AI_POLICY.md` for the stated AI-use policy. The live issue thread for
maintainer comments from OWNER, COLLABORATOR, or MEMBER. The student's
`pr_draft.md` for whether every template section has real content.

**What good looks like:** Every section the PR template explicitly
requires has real content — not a checkbox left unchecked when it
should be checked, not a section heading with no body, not a
placeholder like `[describe your changes here]`. For AI disclosure: if
the policy says anything equivalent to "all AI usage must be disclosed,"
an explicit statement naming the tool and the extent of assistance
appears somewhere in the description. For maintainer direction: if an
owner, collaborator, or member named a culprit, a fix approach, or a
testing request in the thread, the description engages it — even a
brief "I saw your pointer at X and my plan builds on the same location"
counts; silence does not.

**Key traps:**
- A blanket AI-disclosure policy requires a disclosure statement even
  when the policy also says the human must understand the work. Both
  requirements apply.
- A human-voice policy ("comments must be in your own words") does NOT
  require a disclosure statement; it only requires the text reads as
  human-authored.
- A template with no required sections (no PR template, or a template
  that only suggests) does not gate the check — the check passes on
  this dimension by default.
- A classmate's comment (NONE role) in the thread does not count as
  maintainer direction — only OWNER, COLLABORATOR, or MEMBER comments
  do.
