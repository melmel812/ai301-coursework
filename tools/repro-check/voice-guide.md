# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student making my first open-source contributions through a course. I've reproduced (or attempted to reproduce) the bug I'm commenting on, and I'm reporting what I actually found — nothing more. Readers can expect concrete evidence and honest uncertainty: I won't claim to understand code I haven't read or promise a fix I haven't written.

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

## Things I never post

- "I will fix this by [date]" — I don't know that yet
- "Assign this to me" or "please reserve this issue" — open-source issues aren't assigned by request
- "+1" or "same here" without an artifact — assertion is not evidence
- "Obviously" or "clearly" when diagnosing a bug I haven't fully traced
- Guaranteed timelines or outcome promises of any kind
- Apologies for my comment's length or quality — just write it well and post it
