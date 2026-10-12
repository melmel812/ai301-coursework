# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/142

**Branch**

fix/62-redis-health-url

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

No eval run completed. Credit balance was insufficient to run the harness (`claude -p` exited with "Credit balance is too low"). The eval-run.txt in this directory is the placeholder shipped with the starter.

**Package analysis**

pkg-12 (sharkdp/bat#3845, category: unreviewable). My rubric decided **reject**; the gold label is **reject**.

The diff-quality check caught it: the diff carries a commented-out debug eprintln (`// eprintln!("DBG buffer_size = {}", buffer_size);`), a commented-out first attempt with a "keeping for reference" note (`// let mut buffered_lines: VecDeque<(Vec<u8>, usize)> = VecDeque::with_capacity(buffer_size.min(MAX_PREALLOC)); // ^ first attempt: min() still preallocates 8MB`), and a dead function added by the PR (`fn _unused_buffer_probe(cap: usize) -> usize { ... }`) that has no call sites. The commits named "wip", "fix", and "fmt + cleanup" further signal the diff was not cleaned up. My diff-quality check's pass condition is: "The diff contains only the fix and any directly required supporting code; no debug leftovers, no commented-out experiments, no dead functions." All three debris types are present, so it fails. The fix itself is correct and the evidence is decisive — this is a clear unreviewable case where the right change is buried in debug artifacts.

**Check rationale**

From `rubric.md`, the test-evidence check reads:

> | test-evidence | The test-evidence section's before/after output, read against the plan's test plan and the repro's specific failure command and observable artifact. The outcome of the repo's own checks (test suite, lint, CI commands) if the repo's template or contribution guide states they are required. | At least one observable before/after demonstrates that the specific failure the repro showed changed in the expected direction — the same scenario, not a control that was never broken. If the repo's template or contribution guide requires named checks (e.g., `cargo test`, `make lint`, a named CI suite), their outcome must be shown and passing (or honestly explained if a failure is unrelated). Fails if: the before/after is absent; only "tests pass" is stated with no named outcome for the fix; the evidence exercises a control path that was already passing; required repo checks are not shown. | required |

It reads this way because the not-tested failure family has two distinct subtypes that a single pass condition must catch: (1) missing before/after entirely (pkg-10: "verified working on my machine for a full day"), and (2) evidence on the wrong path — a control scenario that was already passing rather than the one the repro showed breaking (pkg-07: only the single-file control re-run, not the two-file repro; pkg-14: GET with header casing rather than the POST Content-Type fix). Requiring "the same scenario, not a control that was never broken" catches subtype 2 specifically. I rejected a simpler pass condition ("before/after shown") because it would have passed pkg-07 and pkg-14, which exercise paths that never broke.

**Trade-offs**

The test-evidence check's pass condition — "the same scenario, not a control that was never broken" — requires the grader to identify which scenario the repro showed failing and then verify the evidence runs that same scenario. For packages where the repro scenario and the evidence scenario are similar but not identical (e.g., same command, different flags), this comparison can be genuinely unclear. I accept that the check may grade unclear (which counts as fail) in ambiguous cases where the scenarios are close but not identical, rather than giving the benefit of the doubt. The calib-04 package is a calibration example of this: the plan's test plan names two failure modes, the evidence re-runs only one. My rubric rejects on decisive-test-evidence because the second named mode is not shown, but a grader who reads "covers the primary repro" as sufficient would accept. I accept this cost: a rubric that passes on partial evidence creates more risk than one that holds borderline cases for honest disclosure.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
