# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68

**Verdict output**

```
I have all the evidence needed. Now let me compile and output the grades.

---

## Grading Summary

**Repo-level facts (apply to all three issues):**
- Archived: No
- Most recent commit: 2026-09-16 (Aburke225) — 4 days before capture date → well within 400 days → **Repo liveness: PASS**
- No releases found (404), but the commit recency alone satisfies the OR condition
- No CONTRIBUTING.md found → silence → **AI policy: PASS**
- All three issues: no assignees, no open PRs, no maintainer-directed claims

---

### Issue #68 — `ZeroDivisionError` when keyword index is empty

| Check | Grade | Evidence |
|---|---|---|
| Repo liveness | **pass** | archived: no; most recent commit 2026-09-16, 4 days ago, well within 400 days |
| Not claimed | **pass** | Assignees: none; linked PRs: none; yulijasso's claim comments are student-only (Path Review house rule: student claims don't block) |
| Bounded scope | **pass** | Single bug: add empty-corpus guard in `KeywordSearcher.index()`, remove xfail marker on one test |
| AI policy | **pass** | No CONTRIBUTING.md found; silence passes |
| Newcomer signal | **pass** | Labelled `good first issue`; body names the xfail test as the acceptance criterion |

**Verdict: accept**

---

### Issue #60 — Faithfulness checker crashes when `text: None`

| Check | Grade | Evidence |
|---|---|---|
| Repo liveness | **pass** | archived: no; most recent commit 2026-09-16, 4 days ago, well within 400 days |
| Not claimed | **pass** | Assignees: none; no open PRs; no comments at all |
| Bounded scope | **pass** | Single bug: replace bare `.get("text", "")` with a guard that coerces `None` to `""`; body includes failing test reference |
| AI policy | **pass** | No CONTRIBUTING.md found; silence passes |
| Newcomer signal | **pass** | Labelled `good first issue`; body references a failing test case as acceptance criterion |

**Verdict: accept**

---

### Issue #73 — README and `.env.example` disagree about LLM API key

| Check | Grade | Evidence |
|---|---|---|
| Repo liveness | **pass** | archived: no; most recent commit 2026-09-16, 4 days ago, well within 400 days |
| Not claimed | **pass** | Assignees: none; no open PRs; no comments |
| Bounded scope | **pass** | Two-file docs alignment: add `OPENROUTER_API_KEY` to `.env.example`, reconcile README; 1–2 hour estimate in body |
| AI policy | **pass** | No CONTRIBUTING.md found; silence passes |
| Newcomer signal | **pass** | Labelled `good first issue`; body states exact files to change and expected outcome |

**Verdict: accept**

---

### Ranked accepted candidates (by fit profile)

1. **#60** *(top pick)* — Python bug fix touching one method, a failing test already in the repo defines done, zero competing student activity. The student's Python/ML background and preference for concrete, test-anchored tasks maps perfectly here.
2. **#68** — Also an excellent Python bug fix with a pre-existing xfail test as the finish line. Slightly lower because yulijasso's detailed claim comments signal active student interest (harmless under house rules, but worth noting).
3. **#73** — Docs-only change; fits the fit profile's "docs fix is ideal" note, but provides the least codebase-reading practice of the three, which is the main thing the student said they want to build.
```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "Repo liveness", "grade": "pass", "evidence": "archived: no; most recent commit 2026-09-16 (4 days before grading date), well within 400-day window"},
      {"name": "Not claimed", "grade": "pass", "evidence": "Assignees: none; no open linked PRs; no comments of any kind"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single bug: coerce None to empty string in faithfulness checker's context join; body references a failing test case as acceptance criterion"},
      {"name": "AI policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found in repo; silence passes per rubric"},
      {"name": "Newcomer signal", "grade": "pass", "evidence": "Labelled 'good first issue'; body references failing test case as explicit acceptance criterion"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "Repo liveness", "grade": "pass", "evidence": "archived: no; most recent commit 2026-09-16 (4 days before grading date), well within 400-day window"},
      {"name": "Not claimed", "grade": "pass", "evidence": "Assignees: none; no open linked PRs; yulijasso's claim comments are student-only — Path Review house rule exempts these"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Single bug: add empty-corpus guard in KeywordSearcher.index() and remove one xfail marker; scope fully stated in body"},
      {"name": "AI policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found in repo; silence passes per rubric"},
      {"name": "Newcomer signal", "grade": "pass", "evidence": "Labelled 'good first issue'; body names the xfail test as the acceptance criterion"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "Repo liveness", "grade": "pass", "evidence": "archived: no; most recent commit 2026-09-16 (4 days before grading date), well within 400-day window"},
      {"name": "Not claimed", "grade": "pass", "evidence": "Assignees: none; no open linked PRs; no comments"},
      {"name": "Bounded scope", "grade": "pass", "evidence": "Two-file docs alignment: add OPENROUTER_API_KEY to .env.example, reconcile README; 1–2 hr estimate confirms narrow scope"},
      {"name": "AI policy", "grade": "pass", "evidence": "No CONTRIBUTING.md found in repo; silence passes per rubric"},
      {"name": "Newcomer signal", "grade": "pass", "evidence": "Labelled 'good first issue'; body states exact files to change and expected outcome"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

20/20 on the first and only run. From eval-run.txt:
`agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

Issue: **issue-12** (bookwyrm-social/bookwyrm#1133), category: policy.

My rubric's decision: **reject** (AI policy check fails). Gold label: **reject**. Agreement: yes.

The contribution policy line in the bundle reads: "Meaningful human interaction is the whole point of BookWyrm. We do not accept AI-generated code or documentation. If you are unsure how something in BookWyrm works, please ask for help – we are keen to help other humans to understand and contribute to the project."

My AI policy check's pass condition states: "Pass unless the policy contains an outright ban on AI-generated code or documentation (e.g., 'we do not accept AI-generated code or documentation')." The exact phrase "We do not accept AI-generated code or documentation" matches my fail condition literally. The rubric correctly produced reject on that single signal without needing to look at any other check. The issue itself is otherwise clean — the repo is active, the issue is unclaimed, the scope is bounded — so the policy check is the only thing that changes the verdict here. The rubric read the issue exactly as the gold label intended.

**Check rationale**

Quoted from the current rubric.md in tools/issue-select/:

> Pass unless the policy contains an outright ban on AI-generated code or documentation (e.g., "we do not accept AI-generated code or documentation"). Silence (no policy) passes. Requirements to disclose AI use, personally understand changes, test, or have work reviewed pass. Only an explicit prohibition fails.

I wrote the check this way because the evidence guide distinguishes three tiers: outright bans, conditions, and silence. A ban makes contribution impossible regardless of code quality; conditions are terms to follow, not blockers. The example phrase ("we do not accept…") sets a concrete string pattern for what counts as a ban so that the check cannot drift toward treating disclosure requirements or testing requirements as bans. The "silence passes" clause is necessary because most repos say nothing about AI, and treating silence as a ban would reject too many valid issues.

**Trade-offs**

The "only an explicit prohibition fails" rule means repos that strongly discourage AI use — but stop short of a ban — pass the AI policy check. Issue-10 (tldr-pages megaissue) has this kind of policy: "strongly discourages generative AI for new pages; PRs suspected of being made wholly or partly with generative AI without human review are closed." That is a conditional restriction, not a ban, so my AI policy check passes it. Issue-10 is still correctly rejected, but on scope (megaissue/tracking list), not on policy. If I tightened the AI policy check to also fail on "strongly discourages," issue-10 would be double-rejected — no change in the verdict — but it would incorrectly reject other repos that merely say "please review AI output carefully," which are valid contribution targets for this course. The current threshold correctly accepts those and only blocks true bans.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** Issue #68 is a Python bug in the RAG keyword-search module — exactly the domain I am studying in AI301. The fix is bounded to one file (`rag/retriever/keyword_search.py`) and the body estimates 2–4 hours of work, which fits the time I can give this week. I have used BM25 in coursework and understand why an empty corpus causes a division by zero in the IDF calculation, so I will not be starting from scratch conceptually.

2. **What the verdict identified correctly, and what I weighed that the rubric could not.** The rubric correctly accepted all three issues and ranked #60 first based on the fit profile. What the rubric ranked on fit alone could not weigh is domain depth: both #60 and #68 are single-file Python bugs with a pre-existing failing test, but #68 touches BM25 keyword search — a retrieval concept directly covered in AI301 — while #60 is a `None`-coercion guard with less connection to the course material. The xfail marker in #68's body also makes the acceptance criterion unusually explicit: the test tells me exactly when I am done.

3. **Anticipated difficulty in claiming it.** Under the Path Review house rule, student claim comments (including yulijasso's) do not block the issue, so there is no race. The technical difficulty is low: guard `index()` against an empty corpus the same way `search()` already does, then remove the xfail marker. The main risk is unfamiliarity with the project's test runner setup, which I will address in Unit 2 when I reproduce the issue locally before claiming.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
