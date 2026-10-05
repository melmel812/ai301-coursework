# Procedure: how this skill grades a plan package

## Read order

Read in this order before grading anything. The order matters: reading repro evidence before the plan prevents the plan's confident framing from shaping what you see in the evidence.

1. Read the **repo-facts block**. Note the contribution policy field verbatim — specifically whether it contains a blanket AI disclosure requirement (any phrase like "all AI usage must be disclosed" or "AI-assisted contributions must disclose"). Write it down: "AI policy: blanket / non-blanket / none."
2. Read the **issue section** (issue description + thread highlights). Note the failure behavior the issue reports. For each comment from an OWNER, COLLABORATOR, or MEMBER in the thread highlights: write down what they said verbatim if they named a culprit, a fix location, a patch, or a testing request. These are the maintainer directions the plan comment must engage.
3. Read the **repro evidence block**. Note: (a) the specific failure shown in the artifacts, (b) every control run and what it rules out — write each as "control X: showed Y, which means mechanism Z is/is not the trigger", (c) the command and output that best demonstrate the failure. Pay close attention to controls that show the named mechanism working correctly in isolation or in a non-triggering context.
4. Read the **candidate plan**. Note the stated cause, the in-scope/not-in-scope line, the files or areas named, the approach, and the test plan.
5. Read the **candidate plan comment**. Note what it says about the diagnosis, approach, thread engagement, and AI disclosure.

## Evidence gathering

For each rubric check, gather exactly the following before grading:

**diagnosis-grounded:**
- Pull the plan's stated cause from the Diagnosis section or the plan's first substantive paragraph.
- Pull each control run from the repro evidence block. For each control, write: "control X: showed Y, which means mechanism Z is/is not the trigger."
- Key question: does any control run show the named mechanism working correctly in an adjacent or isolated context, while only failing in the specific triggering context? If yes, the diagnosis blaming the mechanism itself is contradicted — the defect is in the calling context or surrounding infrastructure, not in the mechanism.
- Record: does any control run rule out the plan's stated mechanism? If the control shows the mechanism works in isolation but not in the triggering context, record that as "mechanism works in isolation, failure is context-specific."

**scope-bounded:**
- List all distinct work items the plan proposes (one bullet per separable change: a file change, an abstraction, an upgrade, a new feature, a CI job).
- For each item, ask: would this item be required to fix the reproduced failure? Mark it "required" or "extra."
- Record: how many items are "extra"?

**executable:**
- Find the files, modules, or code areas the plan explicitly names.
- Find the plan's stated approach or steps.
- Record: (a) the specific file(s), module(s), or code area(s) named (or "none"), (b) one sentence describing the approach (or "none stated"), (c) whether exact function names or implementation details are deferred, and if so, whether there is active debugging already underway that makes the deferral reasonable.

**decisive-test:**
- Find the test plan section (or the absence of one).
- Cross-reference it against the repro evidence's failure command and artifact — what would "fixed" look like?
- Record: the test plan's stated check (verbatim), and whether it is (a) a specific runnable command with observable outcome, (b) "run the test suite" with no specific outcome for the fix, (c) a subjective description, or (d) absent.

**thread-aware:**
- List each maintainer direction noted from the thread highlights (step 2 of Read order).
- Check the plan comment text: does it mention each direction, build on it, or depart from it with a stated reason?
- Check the AI policy (step 1 of Read order): is it blanket? Is there a disclosure statement in the plan comment?
- Record: maintainer directions present (or "none"), whether each is engaged in the comment, AI policy type, and whether a disclosure appears.

## Check execution

Execute checks in this order: diagnosis-grounded, scope-bounded, executable, decisive-test, thread-aware. Work from gathered evidence only — do not re-read the whole package per check.

**diagnosis-grounded:** Compare the plan's stated mechanism against each control run in the repro evidence.
- If a control run shows the mechanism works correctly in isolation or in a non-triggering context, and the failure only occurs when called from a specific surrounding context (e.g., a condition evaluator, a different calling path), then a diagnosis that blames the mechanism itself (rather than the surrounding context or context-setting infrastructure) is contradicted — grade fail. The mechanism "works at top level but not inside conditions" means the calling context is the defect, not the mechanism.
- If no control run rules out the named mechanism, grade pass.
- If the diagnosis is vague or incomplete but not contradicted, grade pass.
- If the repro evidence block is missing entirely, grade unclear.

**scope-bounded:** Count the "extra" items from gathering. If one or more "extra" items represent substantial, separable work (not a one-line consequence of the fix), grade fail. If every item is necessary to close the reproduced failure, grade pass. Edge: a not-in-scope line that explicitly defers a natural extension counts as bounded — the plan is opting out of extra work, which is the right call.

**executable:** 
- If the plan names zero files, modules, or code areas, grade fail.
- If the plan names at least one file, module, or code area AND describes a specific approach (what change to make, not just where to look), grade pass — even if exact function names are deferred because active debugging is already underway.
- If a module or area is named but the approach is entirely "investigate and see what I find" with no current direction, grade fail.
- If every key implementation decision is explicitly deferred to build time with no stated current direction, grade fail.
- The distinction: "I'll pin the exact function after tracing the debug log I have working" passes (area known, approach known, active progress); "I'll figure out where the bug is once I look" fails.

**decisive-test:** If the test plan names a specific runnable check tied to the reproduced failure (a command, an expected exit code, an observable state change), grade pass. If the test plan says only "run the test suite" or "run cargo test" with no expected outcome for the specific fix, grade fail. If the test plan is entirely absent, grade fail. If the test plan describes only a subjective feeling or "nothing else should break," grade fail.

**thread-aware:** If no maintainer directions are present in the thread highlights, go directly to AI disclosure. If maintainer directions are present and the plan comment is silent about them (no mention, no departure with reason, no engagement), grade fail. If the plan comment engages each direction — even to say "I am taking a different approach because..." — grade pass on this dimension. Then check AI disclosure: if the policy is blanket and no disclosure statement appears in the plan comment (in eval mode, all packages are treated as AI-assisted), grade fail. If the policy is non-blanket or absent, grade pass on disclosure. The check fails if either dimension fails.

If evidence for a check is genuinely absent (no test plan section, no scope line, no thread highlights), apply the rule for the absent case stated above — do not grade unclear simply because the section is missing.

## Verdict assembly

1. List each check name and its grade: pass, fail, or unclear.
2. Treat every unclear grade as fail (per the verdict rule).
3. If all five checks are pass: verdict is **accept**.
4. If any check is fail (or unclear): verdict is **reject**. In the readable summary, quote the evidence line from the first failing check as the deciding reason.
5. In the JSON block, include all five checks with their grades and evidence lines, then the verdict.
