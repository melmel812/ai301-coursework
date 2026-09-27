# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| evidence-present | The repro report's body: every output excerpt, log block, command transcript, or screenshot shown | The report contains at least one concrete artifact (command output, error message, log excerpt, or screenshot) that shows observed behavior; "I can confirm" or "same here" with no artifact fails | required |
| behavior-matches-issue | Each artifact in the repro report compared against the issue's stated failure mode (its error type, exit code, or observable symptom) | The artifact(s) demonstrate the same failure mode the issue describes — same error type, same code-path failure, same observable symptom — or the report explicitly states it could not reproduce and shows what was observed instead; an artifact showing a different error on a different code path, or a confident reproduction claim backed by the wrong behavior, fails | required |
| environment-recorded | The repro report's environment record: OS name and version, tool or library version(s) named in the issue, any driver or backend the issue calls out | The repro report names at minimum the OS and the main tool's version; if the issue is driver-, backend-, or architecture-specific, that detail is also named; no environment record at all fails | required |
| steps-followable | The repro report's steps section and any external resources referenced | Each step is a concrete, runnable action with enough detail for a stranger to execute it without guessing; the starting state is specified; references to the issue's own reproduction script or example are followable when the issue context in the package provides that information; steps that point exclusively to private or third-party-inaccessible resources (company repos, unshared configs) fail; steps that omit a qualifier (driver, flag, or option) the issue identifies as the trigger fail | required |
| honest-outcome | The repro report's stated conclusion compared against its own artifacts | The stated result matches what the artifacts actually demonstrate; silent version deviations (using a different version without acknowledging it and presenting the result as evidence about the reported version) fail; a stated conclusion that describes behavior absent from the shown artifacts fails; an explicit cannot-reproduce that names what differed and what was tried passes | required |
| claim-specific | The claim comment | The claim comment names the specific bug or symptom from the issue (not just "this issue" or "I'd like to contribute"), names a concrete next step the claimer will take, and does not promise a guaranteed fix timeline or "assign me" self-assignment; "+1" or generic assignment requests with no specificity fail | required |
| disclosure-if-required | The repo-facts block's contribution policy field and the text of the claim and repro comments | If the repo's stated policy is a blanket AI-disclosure requirement (e.g., "all AI usage must be disclosed" or "AI-assisted issues must disclose the tool and extent"), the comments must include an explicit disclosure; in eval mode, treat all packages as AI-assisted for this check, so absence of disclosure under a blanket policy fails. If the policy requires only that comments be written by humans in their own words (a quality standard, not a disclosure requirement), a human-voiced comment satisfies it without a disclosure statement. If there is no stated AI policy, this check passes automatically | required |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. `unclear` counts as fail: evidence you cannot verify is not evidence ready to post.
