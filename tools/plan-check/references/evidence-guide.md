# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**In an eval bundle:** The plan's stated cause lives in the "Diagnosis" section of the candidate plan, or in the plan's opening paragraph when no heading is present. The repro evidence that must agree with or contradict it lives in the "Repro evidence" block — especially the control runs: each control isolates one variable and rules in or out a specific mechanism.

**In live mode:** The plan's cause is in the student's draft plan.md, Diagnosis section. The repro evidence is the student's posted repro comment on the issue thread (their week-2 proof).

**What good looks like:** The stated cause predicts behavior that the repro's artifacts actually show, and is not eliminated by any control run. Good: "the refresh scope misses the branch-commits context" when the repro shows the view fails to refresh until re-entered. Bad: "the tokenizer is too strict" when a control run shows the same tokens parse fine without the triggering flag — the control eliminates tokenizer strictness as the cause.

**Key trap:** A polished confident diagnosis that contradicts a control run in the repro evidence fails, even if it matches the thread discussion. The package's own repro evidence is the ground truth.

## Scope

**In an eval bundle:** The plan's scope lives in an explicit "Scope" section (with in-scope / not-in-scope lines), or implicitly in the full list of proposed changes if no scope section exists. The issue description names what the reporter asked for — compare the plan's changes against that.

**In live mode:** The plan's scope is in the student's draft plan.md, Scope section.

**What good looks like:** One bounded change targets the reproduced failure. The not-in-scope line names one or more natural extensions that the plan explicitly defers. Good: "not in scope: redesigning wide-char wrapping at tiny widths." Bad: a plan whose "Changes" list includes a dependency upgrade, a cross-runtime abstraction, a CI matrix job, and an error-surfacing fix — four separable work items when the issue only asked for the error to stop being silent.

**Key trap:** A plan that bundles substantial work the issue never asked for fails scope-bounded even if every item is reasonable. "While we're here" logic is scope creep. A plan that is terse and fixes only the issue passes even if it looks thin.

## Executability

**In an eval bundle:** Files or code areas live in the plan's "Files" section, or are named inline in the approach or steps. The approach lives in the "Approach" or "Steps" section, or is described in the plan body.

**In live mode:** Files are in the student's draft plan.md, Files section. Approach is in the Approach section.

**What good looks like:** At least one specific file path or module is named, and the approach says what change will be made — not just where to look. Good: "`crates/cli/src/decompress.rs` — append `--` before the file path in the command builder." Bad: "Profile starship on Windows to find the slow parts of the git modules" (no files, no chosen fix, just an investigation plan).

**Key trap:** A plan that names only the area to investigate ("the undo stack somewhere in the editor code") without naming a file or a concrete change fails executable. Naming a file AND saying "investigate it" also fails if the approach defers every implementation decision.

## Test plan

**In an eval bundle:** The test plan lives in the plan's "Test plan" section. The repro evidence's failure command and observable artifact (the command that produced the bug, the expected vs. actual output) are what the test plan must connect to.

**In live mode:** The test plan is in the student's draft plan.md, Test plan section.

**What good looks like:** The test plan re-runs the repro scenario (or a derived check) and states a specific observable outcome. Good: "re-run the repro command; expect drawn output and exit 0" — ties back to the repro's `echo $? → 101`. Good: "at step 3 the color must flip without leaving the view" — names the exact repro moment and what "fixed" looks like. Bad: "the prompt should feel fast in big repos on Windows, and starship timings should look much better" — no runnable check, no measurable threshold.

**Key trap:** "Run the full test suite and make sure nothing regresses" fails decisive-test because it names no observable outcome for the fix itself. The test suite passing means nothing broke; it does not verify the fix works.

## Honesty

**In an eval bundle:** Stated unknowns and risks live in the plan's Risks/Unknowns section, or are flagged inline. A mid-build deviation lives in a "Deviations" section at the end of the plan (added after the build begins, not before).

**In live mode:** Risks are in the student's draft plan.md. Deviations are recorded in the plan after the build begins.

**What good looks like:** The plan names what it does not yet know without dressing it up as certainty. Good: "Risk: I have not yet measured the per-print cost of the generation comparison; if it shows up in the benchmark I will move the check." Bad: "Should be doable in a few evenings" (no stated uncertainties, no named risks). A deviation recorded honestly in plan.md after the build differs from the plan is good evidence; a deviation that only shows up in the diff is not.

**Note:** Honesty is not a separate rubric check in this rubric (there is no standalone "honesty" check), but the evidence here informs diagnosis-grounded (a plan that presents speculation as certainty may contradict the evidence) and executable (a plan that defers every decision to build time fails executable).

## Comms

**In an eval bundle:** The plan comment lives in the "Candidate plan comment" section. Thread highlights (maintainer signals) live in the "Thread highlights" section — look for comments from OWNER, COLLABORATOR, or MEMBER. The AI disclosure policy lives in the repo-facts "contribution policy" field.

**In live mode:** The plan comment is the student's draft comment.md. Thread highlights are the live issue thread on GitHub. The AI policy is in the repo's CONTRIBUTING.md or AI_POLICY.md, accessible via `gh` or the web.

**What good looks like:** Thread-aware: the plan comment mentions or engages every concrete maintainer direction in the thread highlights. If the owner identified a culprit file, the plan comment names it or explains the departure. If the owner asked for testing and posted a patch, the plan comment references that direction. Engagement does not mean agreement — a comment that says "I saw the owner's diagnosis and am taking a different approach because X" is engaged; a comment that ignores the owner's direction entirely is not.

AI disclosure: a blanket policy ("all AI usage must be disclosed," "AI-assisted issues and comments must disclose") requires an explicit statement in the plan comment naming the tool and extent. A human-voice policy ("comments must be written by humans in their own words") is satisfied when the comment reads as the student's own words — no disclosure statement is needed. No stated policy: the check passes automatically on disclosure. In eval mode, treat every package as AI-assisted for disclosure purposes.
