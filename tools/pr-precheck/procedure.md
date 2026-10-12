# Procedure: how this tool grades a PR package

## Read order

Read in this order before grading anything. The order matters: reading
the plan before the diff prevents the diff's confident shape from
shaping what you expect the plan to say.

1. Read the **repo-facts block** (in eval) or fetch the repo's
   contribution policy and PR template (in live, via `gh` or the web).
   Note verbatim: (a) every section the PR template requires — write
   each as a required item; (b) whether the contribution policy has a
   blanket AI-disclosure requirement ("all AI usage must be disclosed"
   or similar) — write "AI policy: blanket / human-voice / none"; (c)
   any required checks named (e.g., `cargo test`, `make lint`, a CI
   suite).

2. Read the **issue section** and **thread highlights**. Note the
   failure the issue reports. For each comment from an OWNER,
   COLLABORATOR, or MEMBER: write down what they said verbatim if they
   named a culprit, a fix location, or a testing request. These are the
   maintainer directions the PR must engage.

3. Read the **plan-context block** (in eval) or `plan.md` including any
   `## Deviations` section (in live). Note: (a) the plan's scope
   statement — the in-scope boundary and any explicit not-in-scope
   items; (b) the files and code areas the plan names; (c) the plan's
   test plan — the specific check and expected outcome; (d) any
   deviation notes (what changed after the build began and why).

4. Read the **candidate PR diff** (in eval the diff block; in live
   `git diff main...HEAD`). Note: (a) every file changed; (b) every
   semantic hunk — what it does; (c) any debug statements, commented-out
   code, dead functions, or formatting-only hunks; (d) commit messages.

5. Read the **candidate PR title and description** (in eval the Title
   and Description sections; in live `pr_draft.md`). Note every claim
   the description makes about what the diff contains or does not contain.

6. Read the **test-evidence section** (in eval the Test evidence section;
   in live `test_evidence.md`). Note: (a) whether a before/after is
   shown; (b) the specific scenario exercised; (c) the observable
   outcome stated; (d) which repo checks are shown and their outcome.

## Evidence gathering

For each rubric check, gather exactly the following before grading.
Work from notes taken in the read-order phase.

**plan-fidelity:**
- List every changed file and semantic hunk from the diff (step 4).
- For each, ask: is this file/hunk named in the plan's scope, or
  entailed by it (a directly required supporting file like a test or
  changelog entry the plan mentioned), or covered by a deviation note?
  Mark each as "in-scope", "deviation-covered", or "extra."
- List the description's claims about what the diff contains (step 5).
  For each claim, ask: does the diff show this? Mark each "confirmed"
  or "contradicted."
- Record: (a) how many hunks are "extra" and what they do; (b) any
  description claim that is contradicted (overclaim) or missing from
  the description when it should be (underclaim that creates
  misleading impression of completeness).

**test-evidence:**
- Pull the plan's test plan from step 3: the specific check and
  expected outcome.
- Pull the repro's failure command and observable artifact from step 2
  (the scenario that demonstrated the bug).
- From step 6, record: (a) the scenario the before/after exercises —
  is it the same scenario the repro showed failing, or a different
  (control) path? (b) the observable outcome stated before and after;
  (c) which required repo checks are shown and whether they passed.

**diff-quality:**
- From step 4 notes, list every instance of: debug print statements;
  commented-out code blocks; dead functions or dead imports added by
  this PR; formatting-only or whitespace-only hunks; commit messages
  containing wip, fix, fmt, misc, cleanup, or similar.
- Record: presence (yes/no) of each debris type.

**standards-comms:**
- From step 1 notes, list each required template section.
- From step 5, note whether each required section has real content
  (not a placeholder or blank).
- From step 1, recall the AI policy type. From step 5, check whether
  an explicit AI disclosure statement appears (in eval mode, treat
  every package as AI-assisted).
- From step 2, list maintainer directions. From step 5, check whether
  each direction is engaged (mentioned, built on, or explicitly
  departed from with a reason).

## Check execution

Execute checks in this order: plan-fidelity, test-evidence,
diff-quality, standards-comms. Work from gathered evidence only — do
not re-read the whole package per check.

**plan-fidelity:**
- If one or more hunks are "extra" (not in scope and not deviation-
  covered) and represent more than a trivial mechanical consequence of
  the fix, grade fail. A new config flag, a refactored helper, a
  renamed event, or reworded documentation the plan did not name are
  all "extra."
- If the description overclaims (claims the diff contains something it
  does not) or underclaims in a way that obscures a shortfall (the
  plan promised two deliverables and the diff has one, but the
  description says "implements the plan in full"), grade fail.
- If the diff delivers only a subset of the plan's stated scope AND a
  deviation note in `plan.md` and in the description both acknowledge
  this, grade pass — honest deferred scope is not drift.
- If evidence is genuinely absent (no plan-context block in eval,
  no `plan.md` in live), grade unclear.

**test-evidence:**
- If the before/after is absent entirely, grade fail.
- If the before/after exercises a path that was already passing (a
  control, not the broken scenario), grade fail — the evidence must
  demonstrate the specific failure changed.
- If only "tests pass" or "cargo test passes" is stated with no named
  observable outcome for the fix itself, grade fail.
- If the repo's template or contribution guide names required checks
  and they are not shown, grade fail. If they are shown and passing,
  that dimension passes. A required check shown as failing must be
  explained in the description as unrelated; unexplained failures fail.
- If the before/after is shown, ties to the repro scenario, and states
  a specific observable outcome matching the plan's test plan, grade
  pass.
- If evidence is genuinely absent (no test-evidence section), grade
  fail — absence of evidence is not unclear.

**diff-quality:**
- If any debug statement (eprintln!, println!, console.log, etc.) is
  present in the diff's non-test code, grade fail.
- If any commented-out code block is present (not a doc comment or a
  test comment that names the issue), grade fail.
- If any dead function or dead import added by this PR is present,
  grade fail.
- If any formatting-only or whitespace-only hunk unrelated to the fix
  is present, grade fail.
- If all commit messages suggest work-in-progress state (wip, fix,
  fmt, misc) AND the diff shows the corresponding debris, grade fail.
  Commit names alone (without debris) are not sufficient to fail.
- If none of the above apply, grade pass.

**standards-comms:**
- For each required template section: if it has real content, mark
  pass; if empty or placeholder, mark fail. If any section fails, the
  check fails.
- For AI disclosure: if the policy is blanket and no explicit
  disclosure statement appears in the description, grade fail. If the
  policy is human-voice (no blanket statement required), pass on
  disclosure so long as the text reads as human-authored. If no policy,
  pass on disclosure.
- For maintainer direction: if any direction is present in the thread
  highlights and the description is silent about it (no mention, no
  engagement, no departure with reason), grade fail. If engaged or no
  directions present, pass.
- If all three dimensions pass, grade pass. If any fail, grade fail.
- If the package has no template, no policy, and no maintainer
  directions, grade pass.

## Verdict assembly

1. List each check name and its grade: pass, fail, or unclear.
2. Treat every unclear grade as fail (per the verdict rule).
3. If all four required checks are pass: verdict is **accept**.
4. If any check is fail (or unclear): verdict is **reject**. In the
   readable summary, quote the evidence line from the first failing
   required check (in rubric order: plan-fidelity, test-evidence,
   diff-quality, standards-comms) as the deciding reason.
5. In the JSON block, include all four checks with their grades and
   evidence lines, then the verdict.
