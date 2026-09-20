# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo liveness | `archived:` flag, `last 5 default-branch commits` dates, and `latest release:` under Repo facts | `archived: no` AND (the most recent of the last 5 default-branch commits is dated within 400 days of the capture date, OR the latest release is dated within 400 days of the capture date). Unclear or absent evidence fails. | required |
| Not claimed | `assignees:` and `linked PRs:` fields under Repo facts; comments from Owner/Member/Collaborator authors in the thread | No assignee listed AND no open linked PRs. Additionally fail if a maintainer comment in the thread explicitly directs the work to a named contributor (e.g., "we are only accepting X's PR"). A maintainer saying "feel free to try" does not fail this check. Closed/merged PRs and claim comments from contributors without a maintainer badge do not fail this check. | required |
| Bounded scope | Issue title, body, comment thread, and `linked PRs:` list | Pass only if ALL of the following hold: (1) The issue is not an umbrella, megaissue, or tracking list — its body is not primarily a numbered or bulleted list of sub-issues intended to be split into separate work. (2) The thread does not show an active multi-year design debate with competing proposals that no maintainer has resolved, AND the issue has not accumulated 2 or more closed/unmerged linked PRs without a subsequent maintainer comment explicitly inviting new contributions. (3) If the issue is a feature request, it has sufficient spec to implement without a core product decision still unresolved — no key design elements are left "TBD" or delegated to the contributor to decide. | required |
| AI policy | `contribution policy:` line under Repo facts | Pass unless the policy contains an outright ban on AI-generated code or documentation (e.g., "we do not accept AI-generated code or documentation"). Silence passes. Requirements to disclose AI use, personally understand changes, test, or have work reviewed pass. Only an explicit prohibition fails. | required |
| Newcomer signal | Issue labels in the header, author badge (Owner/Member/Collaborator), and issue body | Issue carries a `good first issue` or `help wanted` label, OR was opened by a maintainer (Owner/Member/Collaborator badge), OR includes explicit acceptance criteria or a "where to start" pointer in the body | preferred |

## Verdict rule

Accept if every required check passes. Preferred checks never change the verdict; they rank accepted issues (an issue that passes more preferred checks is a better fit). Unclear evidence on any required check counts as fail.
