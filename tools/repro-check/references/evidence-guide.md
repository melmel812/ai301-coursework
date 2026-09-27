# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**In an eval bundle:** Lives in the repro report's opening section — look for an explicit "Environment:" line or the first paragraph before the steps. Cross-check it against the issue's stated context (the reporter's OS, versions, driver).

**In live mode:** Lives in the student's draft repro comment. Check that the issue's key version identifiers (OS, main tool version, any driver or backend the issue names) appear in the draft.

**What good looks like:** OS name and version are stated, the main tool's version is named, and any qualifier the issue calls out (driver, library version, architecture) is present. "macOS 14.5, HTTPie 3.2.4, multidict 6.6.0" is sufficient. A version deviation from what the issue reports is fine if it is explicitly acknowledged ("the issue was filed against 13.0.0; behavior is unchanged on 15.2.0"). An environment record that names the tool but not the OS, or no environment record at all, fails.

## Steps

**In an eval bundle:** Lives in the repro report after the environment record, usually a numbered list or command block. Check whether any external resource is referenced and whether that resource is public and accessible.

**In live mode:** Lives in the student's draft repro comment. Check whether every step is runnable from the outside: no private repos, no internal tools, no "just do what I did" gestures.

**What good looks like:** Each step is either a copyable command or a UI gesture specific enough to re-execute. The starting state (file to create, command to run first, initial configuration) is described. Steps that omit a qualifier the issue identifies as the trigger (the `--replace` flag, a specific driver, a specific input shape) fail even if the rest of the steps look complete. Steps that point exclusively to a private or inaccessible resource (internal monorepo, company-specific config) fail.

## Behavior shown

**In an eval bundle:** Lives in the artifacts embedded in the repro report: command output blocks, error messages, log excerpts, terminal captures. The issue context section names the failure mode to match against.

**In live mode:** Lives in the student's draft repro comment. The issue thread and the issue body name what behavior the artifact should demonstrate.

**What good looks like:** The artifact shows the specific failure mode the issue describes — the same error type, the same exit code, the same observable symptom. A log showing the route-ADD loop the issue calls out is good. A log showing the application starting and running normally when the issue reports a crash is not. Honest cannot-reproduce artifacts (showing what the attempt produced and naming what differed) pass: the artifact does not have to show the issue's behavior, but it must show what the attempt actually produced. An artifact showing a different error on a different code path — a compile error when the issue reports a runtime panic, an arg-validation error when the issue reports a capacity overflow — fails even if the artifact looks plausible.

## Honesty

**In an eval bundle:** Lives at the boundary between the repro report's stated conclusion and its artifacts. Read the conclusion, then check whether the artifacts actually support it.

**In live mode:** Lives in the student's draft repro comment. Compare the student's stated result against each artifact shown.

**What good looks like:** The stated result matches what the artifacts show. "Reproduced: the editor window disappears on empty search" passes if the artifact shows the window disappearing. "I confirmed the crash" fails if the artifact shows garbled output with the terminal still alive. Silent version deviations fail: if the report tests on a version the issue was not confirmed on without saying so, the stated conclusion misrepresents the evidence. An explicit cannot-reproduce that names what the attempt produced and why the trigger likely differs passes — honest reporting of a negative result is good evidence.

## Comms

**Claim comment (eval bundle and live):** Reads in the "Candidate claim comment" section. Check: does the comment name the specific bug or symptom (not just "this issue")? Does it name a concrete next step? Does it avoid promising a fixed timeline ("I'll fix it in 2 days") or requesting assignment ("assign me")?

**Repro comment conventions (eval bundle):** The repo-facts block names what the bug-report template asks for (environment, steps, behavior). Check whether the repro report covers those areas. Omitting a field the template explicitly asks for is a signal, but the pass condition is whether the check it falls under (environment-recorded, steps-followable, etc.) passes — not whether the template headings are present.

**AI-use disclosure (eval bundle and live):** Read the repo-facts block's contribution policy field. If the policy states AI assistance must be disclosed (blanket: "all AI usage must be disclosed"), look for an explicit disclosure statement in the claim or repro comments; absence fails. If the policy requires disclosure only when AI was used and the comment is written in human voice with no apparent AI assistance, that satisfies the policy. If the policy is permissive or absent, the check passes automatically.
