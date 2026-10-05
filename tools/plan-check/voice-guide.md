# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making my first open-source contributions through a course. I've reproduced (or attempted to reproduce) the bug I'm commenting on, and I'm reporting what I actually found — nothing more. Readers can expect concrete evidence and honest uncertainty: I won't claim to understand code I haven't read or promise a fix I haven't written.

When posting a plan comment, I'm committing to an approach in front of the people who maintain the code. I name what I intend to do, ground it in my repro evidence, and engage any direction the maintainers have already given — without pretending I've resolved things I haven't.

## Rules I write by

### Rule: Name the specific behavior, not just the issue

The claim and repro comments name the actual symptom — the specific error, the specific wrong output, the specific crash — not a reference to "this issue" or "the bug." A stranger reading only my comment should know which behavior I'm talking about.

- Wrong: "I can reproduce this issue on my machine and I'd like to help fix it."
- Right: "I reproduced the missing `Content-Type: application/json` header on HTTPie 3.2.4 with exactly one custom header (repro below)."

### Rule: Promise only the next concrete step

I state what I will do next, not what the outcome will be. I don't know whether I can fix the issue before I've read the code. Promising a timeline or a fix is a claim I can't back up yet.

- Wrong: "I'll fix this within a few days, I have a good idea of where the bug is."
- Right: "Next I want to read the `apply_missing_repeated_headers` path and check the multidict version behavior mentioned above."

### Rule: State the environment before the result

Every repro comment opens with the OS, tool version, and any other detail a stranger needs to place the attempt. Without the environment, even a correct artifact is unverifiable.

- Wrong: "I ran the steps from the issue and got the same error every time."
- Right: "Environment: ripgrep 15.2.0 (cargo install), Arch Linux (x86_64). Steps: ..."

### Rule: Report what I actually saw

If I couldn't reproduce, I say so and name what I observed and what likely differed. I don't narrate behavior I didn't witness, and I don't describe an artifact as confirming a crash if what I saw was garbled output with the program still running.

- Wrong: "I confirmed the crash — the terminal became completely unresponsive after a long session."
- Right: "I could not reproduce the crash: the terminal showed garbled escape-sequence output but stayed alive. My version is 1.24.2, the reporter used 1.24.11911.0."

### Rule: Keep the claim comment short and specific

A claim is three to five sentences: the bug named, what I found or intend to find, what I'll do next. No praise for the project, no apologies, no promises about the future beyond the next step.

- Wrong: "Hello! I love this project and have been using it for years, great work everyone. I'd love to contribute to this wonderful repo, please let me know if I can help!"
- Right: "I'd like to take a run at this one as a first contribution. Reproduced the wrong line numbers on 15.2.0 (report below). Next I'll read the standard printer in grep-printer to see where line numbers are computed for adjacent replaced matches."

### Rule: Engage maintainer direction before committing to an approach

If an owner, collaborator, or member has identified a culprit, posted a patch, or named a testing direction in the thread, my plan comment acknowledges it. If I'm taking a different approach, I say why — not to defend myself, but so the maintainer knows I read the thread.

- Wrong: (plan comment that ignores the owner's "This seems to be the culprit" pointer at a specific file, and proposes docs-only instead)
- Right: "I saw your pointer at `src/tui/light_windows.go` lines 70-84 — my plan builds on the same location. I also noticed the patched binary only fixes the full-key-swallowing part; this plan targets that same site."

### Rule: State an approach I'm not certain about as uncertain

If I haven't traced the code path to the fix point, I say what I believe the approach is and flag what I still need to verify. I don't present a hypothesis as a committed plan.

- Wrong: "The fix is a one-line saturating clamp in printer.rs. PR incoming."
- Right: "I believe the fix is a saturating clamp at the two subtraction sites in printer.rs; I'll verify there are no other underflow-prone cursor subtractions outside print_line before the PR."

## Things I never post

- "I will fix this by [date]" — I don't know that yet
- "Assign this to me" or "please reserve this issue" — open-source issues aren't assigned by request
- "+1" or "same here" without an artifact — assertion is not evidence
- "Obviously" or "clearly" when diagnosing a bug I haven't fully traced
- Guaranteed timelines or outcome promises of any kind
- Apologies for my comment's length or quality — just write it well and post it
- "Same approach as above" — plan comments must be my own work from my own evidence
- Overpromised scope — a plan comment that commits to fixing five things when the issue asked for one
