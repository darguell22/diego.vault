# Claude Code Course Eater Prompt

Paste-ready prompt for Claude Code on laptop ORTIZ (Claude Pro). Purpose: let
an agent "take" a course Diego already owns — transcribe the lessons locally
for $0, extract the usable techniques as structured skill notes, audit them
against the workflows Diego already runs, and produce ONE ranked integration
plan. Inspired by the @mr.paidsocial TikTok (ChatGPT agent mode doing this),
rebuilt on the stack Diego already pays for: Claude Code + faster-whisper on
his RTX 5070 Ti. No new subscriptions.

How it works, Diego's part: YOU buy the course and YOU log in and get the
materials (download the lesson videos/audio the platform lets you download,
or save the transcripts/captions it provides) into the intake folder below.
The agent never logs in anywhere, never touches credentials, and never goes
around a paywall. Everything the agent produces is personal study notes —
summaries and extracted techniques in your own words, never a verbatim copy
of the course.

Rule baked into the prompt: anything the course tells you to download and run
(scripts, repos, "skill packs") gets flagged for Everest's SkillSpector
security scan first and is never installed or run unreviewed.

---

## The prompt — paste into Claude Code on ORTIZ

```
Build a local "course-eater" pipeline that turns a course I own into
structured skill notes and one integration plan. Work only in
C:\Users\anjel\warren\course-eater\ — create it if missing.

INPUT — I provide the course materials myself. Create an intake\ subfolder.
I will place ONE of these in it:
- lesson video or audio files I downloaded from a course I purchased, or
- transcript / caption files (.txt, .srt, .vtt) the platform provided.
You never log in to any platform, never handle my credentials or passwords,
never bypass a paywall or DRM, and never scrape a site behind a login.
If intake\ is empty, stop and tell me what to put there.

DEPENDENCY RULE — transcription uses faster-whisper, and it is the ONLY
third-party package allowed. First check whether faster-whisper is already
installed (python -c "import faster_whisper"). If it is NOT installed, STOP
and tell me it is missing — do not pip-install anything silently; wait for
my go-ahead. Everything else you write must be Python standard library only.

Build these scripts in the course-eater folder:

1) transcribe.py — for each audio/video file in intake\ with no matching
   transcript yet, run faster-whisper locally on the GPU (device="cuda",
   compute_type="float16"; fall back to CPU int8 only if CUDA fails and
   tell me you did). Model: "medium" by default ("large-v3" if I pass
   --model large-v3). Write one transcript per lesson into transcripts\
   as <lesson-name>.md with a header (source file, duration, model used)
   followed by the timestamped text. Fully offline — no transcription
   APIs, no uploads, no API keys anywhere.

2) extract_skills.py is NOT a script — this step is you, Claude, reading.
   For each transcript in transcripts\, write one file into skills\ named
   <lesson-number>-<short-topic>.md with exactly this structure:
   - Lesson: <title>
   - Key techniques: bullet list, each technique in one line
   - Exact steps / settings: the concrete how-to — menu paths, parameter
     values, prompts, numbers — rewritten as instructions, NOT copied
     paragraphs from the course
   - When to use: the situation each technique is for
   - My notes: leave blank for me
   Summarize and extract technique; never reproduce the course verbatim.

3) audit.py is also you reading, not a script. I may copy my current
   workflow notes into a workflows\ subfolder (my YouTube explainer
   pipeline, my viral-clips workflow, my La Familia Aguacate production
   notes). If workflows\ is empty, say so and do the audit against the
   skill notes alone. Compare every extracted skill against those
   workflows and write audit.md with three sections:
   - Already doing: techniques that match what my workflows already say
   - Gaps: techniques I do not currently use, one line each on what
     they would change
   - Conflicts: places the course contradicts my current workflow —
     state both sides, do not silently pick a winner

4) integration-plan.md — ONE file, built from audit.md. A ranked list of
   changes, easiest first. Each change gets: what to change, which
   workflow it touches, expected payoff (one line), and effort
   (S / M / L). End with a "Do first" section naming the top 3.

HARD RULES:
- Personal study notes only. Nothing here gets published or shared as
  course content; it is my own summarized notes for my own projects.
- If any lesson, resource file, or download link in the course points to
  scripts, GitHub repos, installers, or "skill packs": do NOT download,
  install, or run them. List them in a file flagged-for-scan.md with
  their URLs/paths and a note that they need a SkillSpector security
  scan and Everest's review first.
- No API keys, no credentials, no accounts, no network calls in any
  script you write (transcription is local and offline).

STOPPING POINT: build the pipeline, then process only the FIRST sample
lesson I put in intake\ — transcribe it (or read its transcript), extract
its skill note, and show me: the transcript file, the skill note, and a
one-paragraph summary of what the pipeline produced. Then STOP and wait
for my review before processing the rest of the course. Do not batch-run
a whole course in one go.
```

---

## After the sample lesson checks out

Tell Claude Code: "Process the remaining lessons, then write audit.md and
integration-plan.md." Bring the integration plan back to Everest (paste it
in chat) — Everest does the final judgment call on what actually changes in
the YouTube / viral clips / Aguacate workflows, and the skill notes can be
copied into this vault next to the project notes they belong to.
