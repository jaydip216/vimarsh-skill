# Vimarsh (skill) — UPSC/GPSC answer & essay evaluator

A **Claude Code skill** that evaluates a handwritten UPSC/GPSC mains **answer**
or **essay** — in Gujarati or English. It transcribes your uploaded PDF/image,
optionally researches reference material on the web, scores it against the right
rubric with feedback in Gujarati (English kept for technical terms), tracks your
recurring strengths and weaknesses across evaluations, and answers follow-up
questions.

It runs entirely through your own **Claude subscription** via Claude Code — no
API key, nothing deployed. This is a leaner rebuild of the original Vimarsh web
app as a skill + subagents.

## What's inside

```
.claude/
├── skills/vimarsh/
│   ├── SKILL.md                 # the orchestrator (routes + calls subagents)
│   ├── rubrics/
│   │   ├── answer-rubric.md     # mains-answer scoring (6 parameters)
│   │   └── essay-rubric.md      # essay scoring (different 6 parameters)
│   └── followup.md              # rules for follow-up Q&A
└── agents/
    ├── vimarsh-transcriber.md   # model: sonnet · reads the PDF/image
    ├── vimarsh-researcher.md    # model: sonnet · web-searches references
    └── vimarsh-evaluator.md     # model: opus   · scores against the rubric
memory/
├── answer-profile.md            # recurring patterns — answers
└── essay-profile.md             # recurring patterns — essays
runs/                            # per-evaluation artifacts (git-ignored)
```

**Per-step models** live in each subagent's frontmatter (`model:`). Change one
line to swap a model — e.g. if you hit Opus limits on Pro, set the evaluator to
`model: sonnet`.

## Prerequisites

- [Claude Code](https://claude.com/claude-code) installed and signed in.
- A **claude.ai subscription (Pro or Max)**. Works on Pro; the evaluator uses
  Opus, which Pro allows within its usage limits — see "Model notes" below.

## How to use it (recommended: run from this folder)

1. Open a terminal in this folder:
   ```bash
   cd ~/Desktop/Workspace/vimarsh-skill
   claude
   ```
2. Point Claude at your answer/essay, e.g.:
   > Evaluate this answer: answer.pdf

   **Tip:** put the PDF/image inside this folder first (e.g. drop it in the
   project root or an `inputs/` subfolder) and refer to it by that path. Claude
   Code sandboxes file access to the folder you launched it from, so a file
   sitting elsewhere (like `~/Desktop`) may be blocked or prompt for approval.

   (or just say **"use the vimarsh skill"**). The skill triggers and asks you
   three questions:
   - **Answer or Essay?**
   - **Web research, or just evaluate what's written?**
   - **Feedback language?** (Gujarati + English terms / Gujarati only / English)
3. It transcribes (a multi-page handwritten scan takes a few minutes), then
   researches (if chosen), then shows a scored evaluation.
4. Ask follow-up questions right in the same session ("why did I lose marks on
   structure?"). To grade another piece, just point it at the next file.

Artifacts for each run are saved under `runs/<timestamp>/` (transcript,
references, evaluation). Your recurring patterns accumulate in `memory/`.

### Do I need this "project", or can I install it globally?

Both work — this folder is the **portable, self-contained** option, which is why
it's the recommended one (especially for sharing):

- **As a project (recommended):** keep the folder, run `claude` inside it. The
  skill, subagents, rubrics, and memory are all local to the folder — nothing
  else to set up, and memory stays with the project.
- **Globally (advanced):** copy `.claude/skills/vimarsh` → `~/.claude/skills/`
  and the three files in `.claude/agents/` → `~/.claude/agents/`. Then the skill
  is available in every Claude Code session. Caveat: the SKILL.md uses paths
  relative to the project root for the rubrics and `memory/`, so for a global
  install you'd need to adjust those paths (or still run from a folder that has
  a `memory/` dir). For most people the project approach is simpler.

## Sharing it with someone else (they have Claude Pro)

1. **Reset memory first if you've used it** — `memory/answer-profile.md` and
   `memory/essay-profile.md` hold *your* strengths/weaknesses. Replace their
   contents with the `_No evaluations yet._` seed before sharing so they start
   clean. (`runs/` is git-ignored and won't be shared.)
2. Send them the whole `vimarsh-skill` folder — zip it, or push it to a git repo
   and have them clone it.
3. They just need Claude Code installed and signed in with their Pro account,
   then `cd` into the folder and run `claude` (same as above). No API key, no
   extra setup.

## Model notes

- Defaults: transcribe = `sonnet`, research = `sonnet`, evaluate = `opus`.
- On **Pro**, Opus is available but has tighter usage limits than Max. If your
  friend evaluates many pieces and hits Opus limits, set
  `.claude/agents/vimarsh-evaluator.md` → `model: sonnet`. Quality drops a
  little; reliability and limits improve.

## What changed vs the original web app

- No web UI / sidebar / progress bar — you live in Claude Code; history is your
  sessions plus the `runs/` and `memory/` files.
- Memory is plain editable markdown instead of SQLite, split into answer vs
  essay profiles.
- Adds a dedicated **essay** rubric (the web app only did answers).
- Fixes the transcription reliability issue from the SDK path — this uses Claude
  Code's native file reading directly.
