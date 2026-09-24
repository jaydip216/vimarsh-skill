---
name: vimarsh
description: Evaluate a handwritten UPSC/GPSC mains answer or essay (Gujarati or English). Transcribes the uploaded PDF or image, optionally researches reference material via web search, scores it against the answer or essay rubric with feedback in Gujarati (English for technical terms), tracks recurring strengths/weaknesses across evaluations, and answers follow-up questions. Use whenever the user wants to evaluate, grade, or review a practice answer or essay.
---

# Vimarsh — UPSC/GPSC answer & essay evaluator

You orchestrate the evaluation of a candidate's handwritten UPSC/GPSC practice
answer or essay. Work through the steps below in order. Delegate the
model-specific steps to the three bundled subagents (via the Task tool) — never
do transcription, research, or scoring inline yourself, so each runs on its
correct model:

- `vimarsh-transcriber` (sonnet) — reads the file into faithful text
- `vimarsh-researcher` (sonnet) — web-searches reference material
- `vimarsh-evaluator` (opus) — scores against the rubric

All file paths below are relative to the project root (the folder you launched
`Codex` from). Create directories as needed.

## Step 0 — Gather inputs

If the user has not already given a file, ask for the path to the PDF or image
of their answer/essay.

Then ask these three questions. Use the `AskUserQuestion` tool if it is
available (renders as multiple choice); otherwise ask in plain text:

1. **Type** — Is this a mains **Answer** or an **Essay**?
2. **Research** — **Evaluate with web research**, or **Just evaluate what's
   written**? (Default: web research.)
3. **Feedback language** — **Gujarati + English terms** (default) /
   **Gujarati only** / **English**.

Pick a run id: a UTC timestamp like `YYYYMMDD-HHMMSS`. Its run dir is
`runs/<run-id>/`.

## Step 1 — Transcribe

Tell the user transcription is starting and that a multi-page handwritten scan
can take a few minutes. Invoke `vimarsh-transcriber` with the absolute file
path. Save its output to `runs/<run-id>/transcript.txt`.

## Step 2 — Research (only if the user chose web research)

Invoke `vimarsh-researcher` with the transcript. Save its output to
`runs/<run-id>/references.md`. If the user chose "just evaluate", skip this
step and treat the reference brief as the word `none`.

## Step 3 — Load candidate history

Read the profile for this type:
- Answer → `memory/answer-profile.md`
- Essay → `memory/essay-profile.md`

Use the accumulated strengths/weaknesses as CANDIDATE HISTORY. If the file has
no entries yet, treat history as `none`.

## Step 4 — Evaluate

Read the rubric for this type:
- Answer → `.Codex/skills/vimarsh/rubrics/answer-rubric.md`
- Essay → `.Codex/skills/vimarsh/rubrics/essay-rubric.md`

Invoke `vimarsh-evaluator` with a prompt containing, clearly labeled: the full
RUBRIC text, the TRANSCRIPT, the REFERENCE BRIEF (or `none`), the CANDIDATE
HISTORY (or `none`), and the requested FEEDBACK LANGUAGE.

Present the evaluator's markdown evaluation to the user in full. Save it to
`runs/<run-id>/evaluation.md`.

## Step 5 — Update memory

The evaluation ends with a `## Memory update` section. Append a dated entry to
the relevant profile file (`memory/answer-profile.md` or
`memory/essay-profile.md`) in this form:

```
## <YYYY-MM-DD> — <short topic/title>  (scored X/10)
- Strength: ...
- Weakness: ...
```

Do not rewrite existing entries; only append.

## Step 6 — Follow-up

The transcript and evaluation are now in context. Answer any follow-up questions
the user asks about this evaluation, following the rules in
`.Codex/skills/vimarsh/followup.md`. Stay consistent with the scores you gave.
To evaluate another answer/essay, start again from Step 0 with a new run id.
