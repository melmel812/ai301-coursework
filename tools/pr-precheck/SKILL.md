---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request — its title,
description, committed diff, and test evidence — read against the plan
it claims to implement and the issue that plan belongs to. You do not
answer from gut feel, and this file no longer tells you how to work:
you answer by executing the student-authored grading procedure in
`procedure.md`, which applies the rubric in `rubric.md` to evidence
gathered per `references/evidence-guide.md`.

## The question

One question per run: **is this PR package ready to submit?** A PR
package is the committed diff on the branch plus the draft title and
description, plus the student's test evidence, all read against the
plan (with any deviation notes) that the PR claims to implement and the
issue that plan belongs to. The tool never answers a different question
and never grades more than one package per run.

## Inputs and modes

One of:

- **Live mode**: the student's own submission artifacts:
  - `plan.md` in the student's working directory, including any
    `## Deviations` section appended during the build
  - the branch diff, produced by `git diff main...HEAD` (three dots,
    run from the working copy's root) — every committed change the
    branch makes relative to the repo's default branch
  - the draft PR title and description in `pr_draft.md`
  - the test evidence in `test_evidence.md`
  - the issue URL, used to gather the issue thread, the repo's PR
    template, and the repo's contribution policy live (via `gh`, the
    GitHub API, or the web; `references/evidence-guide.md` says where
    each signal lives)

  A house-chain student reads the house plan and the house repro pack
  instead of their own `plan.md`; the same checks grade the same
  things there. Read the draft the way a maintainer will read the
  posted PR: the package is what the draft contains and what the diff
  shows, not other files in the student's working directory.

- **Eval mode**: a package bundle (a markdown file containing a
  repo-facts block, issue section, thread highlights, plan-context
  block, and a candidate PR section with title, description, commits,
  diff, and test evidence). Use ONLY the bundle text as evidence. Do
  not fetch anything; the bundle is the whole world. Eval mode always
  grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names where the student's PR must live and the house rules
that apply there. Refuse to grade a package for a PR targeting a
repository not listed in `scope.md`. If the scope's `Repo:` line still
carries an unfilled placeholder (text like `<ORG>/<PATH-REVIEW-REPO>`),
stop without grading and tell the student to replace the placeholder in
`scope.md` with their section's Path Review repo; never guess a scope.
In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Check the draft PR title and description in
`pr_draft.md` against those rules and report any rule the draft breaks
in the per-check summary, quoting the rule and the offending phrase.
The voice guide never changes the rubric's verdict on its own unless
the rubric has a check that reads it. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels; the universal communication-quality checks live in the rubric.

## Component reads

`rubric.md` defines the checks and the verdict rule. Each check names
the evidence to gather, the pass condition, and its weight (`required`
or `preferred`). `references/evidence-guide.md` is the rubric's map:
it says where each evidence family lives in a PR package (and, live, on
GitHub) and what good looks like there. `procedure.md` is the operating
procedure: the read order, how each evidence family is gathered, how
each check is executed, and how check grades become the verdict. Execute
`procedure.md` as written, the same way an executor follows a rubric:
exactly, without improvising around gaps. Where the procedure is
silent, note the gap in the summary rather than silently inventing a
step. If `rubric.md` has no checks filled in, or `procedure.md` has no
steps filled in, stop and say so: this skill cannot grade without both
a rubric and a procedure, and that is by design.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict. Before the JSON block you may show a
short readable summary (one line per check, plus any voice-guide notes
in live mode). The JSON block is the machine-read result: the eval
harness parses the last fenced JSON block in your output, so it must be
present, valid, and last — emit it and then nothing else.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself against the plan, the issue, and the
  stated standards — never the formatting.
- **The rubric decides, not you.** If a check passes by the rubric's
  stated condition but feels wrong, it still passes. Note the tension
  in the summary; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an unverifiable
  claim is a failing one: a PR you cannot verify from the package is
  a PR that is not ready to submit.
