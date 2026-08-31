# Coach — a Claude Skill

An open-source coaching skill for Claude, built from a real 8-week training block (a Spartan Super OCR race) and extracted into a reusable procedure. It turns Claude into a persistent coach that reviews your workouts against your own training history, catches drift from your plan, tracks standing injury/constraint flags, benchmarks you before prescribing anything, and schedules sessions on your calendar — instead of just generating a workout plan once and forgetting everything about you the next conversation.

## What this actually is

A single `SKILL.md` file. No code, no server, no new infrastructure. It works because Claude already has:

- **Persistent memory** across conversations (Claude reads/writes markdown files it keeps between sessions)
- **Health data access** (Apple Health or similar, if you've connected it)
- **Calendar access** (Google Calendar, if connected)
- **File uploads** (for workout screenshots, segment exports, benchmark evidence, etc.)

This skill doesn't build any of that — it just gives Claude a disciplined *procedure* for using what's already there, consistently, across a whole training block.

## What it is not

- Not a workout plan generator. You (or Claude, working with you) still design the training block itself.
- Not a fitness tracker. It relies on your existing data connections; it doesn't create new ones.
- Not portable between people. Memory is per Claude account — your training history stays yours, and a fresh account starts genuinely fresh. This skill defines the *shape* new memories should take, not a transplant of someone else's data.

## How setup works

Starting a new goal runs three stages, in order, and the skill won't skip ahead:

1. **Interview** — goal, objective, fitness history, injuries (current and past), logistics, data sources. Asked directly, not assumed from how experienced you sound.
2. **Benchmark** — before the first workout is prescribed, it establishes real current numbers for whatever the plan will center on (reps, load, pace), preferring a safer submaximal test or an uploaded data source over a self-reported guess or a risky true-max test.
3. **Plan** — only now does it discuss the actual training block.
4. **Track** - receive updates in chat or from APIs, adjust training and support you throughout. 

## Installation

Skill installation surfaces differ across Claude products and change over time — check [docs.claude.com](https://docs.claude.com) for the current method for your platform (claude.ai Projects, Claude Code, Claude Cowork, etc.). In general, this means either:

- Adding `SKILL.md` to a Project's knowledge/instructions, or
- Placing it wherever your Claude environment loads custom skills from, so it's available when you start a coaching conversation.

Once installed, just start talking to Claude about your training goal — the skill's Setup section handles onboarding.

## Recommended connections

- **A health/fitness data source** (Apple Health, or whatever your platform supports) — without this, Claude is coaching from your self-reports alone, which the skill will tell you plainly rather than pretend otherwise.
- **A calendar** (Google Calendar or equivalent) — for scheduling sessions with full instructions in the event body, not just a title.

Neither is required to start. The skill degrades gracefully and will ask for manual reporting or screenshots if no data connector is available.

## Other platforms

This is a Claude Skill specifically — `SKILL.md`'s frontmatter and progressive-disclosure loading are an Anthropic-specific mechanism, so there's no "install" step on other assistants. The procedure itself is plain markdown, though, and portable if you're willing to adapt it:

- **A coding agent with real file/shell access** (OpenAI's Codex, or similar) is actually a strong fit — closer to how this skill was designed to run than consumer chat interfaces are. Point the agent at a git repo containing `SKILL.md` plus `/areas/`, `/workouts/`, `/people/` directories, reference `SKILL.md` from that agent's repo-level instructions file (e.g. `AGENTS.md`), and have it commit its changes before the session ends. A fresh session against the same repo picks up right where the last one left off — genuine persistence, using the exact file layout this skill already assumes.
- **ChatGPT** doesn't have an equivalent. Its Memory feature is opaque and model-summarized, not an editable file at a path the coach can point at, and a Custom GPT can't write back to its own Knowledge files from inside a conversation. The closest approximation: paste `SKILL.md`'s body into a Custom GPT's instructions, upload the templates as Knowledge, and manually download/re-upload the area file and workout logs at the start and end of each session — the human becomes the persistence layer instead of the assistant. Wiring up real automatic persistence (a Custom GPT Action calling an external API — Notion, GitHub, a small self-hosted store) is possible but is a small integration project, not a drop-in install.

Either way, Setup's memory check (see `SKILL.md`) is what surfaces this limitation to the athlete directly — it asks what's actually available before running the interview, rather than assuming persistence works and finding out otherwise later.

## Files

- `SKILL.md` — the actual skill. This is what Claude reads.
- `templates/area-template.md` — the shape of the standing goal-summary file the skill creates during Setup.
- `templates/workout-log-template.md` — the shape of each individual session log entry.
- `evals/evals.json` — test prompts + expected behavior, for validating the skill against a real harness (see skill-creator's eval workflow).
- `docs/design-notes.md` — the reasoning behind the memory-structure and procedural choices, for anyone extending or forking this.

## Origin

Extracted from a real coaching relationship conducted entirely in Claude over an 8-week Spartan Super (OCR race) training block — tempo runs, surge bricks, a full dress-rehearsal simulation, fueling protocol testing, and a race outcome — then generalized. The specific plan in that case doesn't ship with this skill; only the coaching *procedure* does.

## Contributing

This is a first cut at generalizing one training relationship. If you use it for a different sport (strength blocks, triathlon, a return from injury), issues and PRs refining the procedure — especially the "Core stance" and "Setup" sections, which are the actual load-bearing parts — are the most valuable kind of contribution. Changes that add sport-specific workout templates are welcome as long as they don't creep into the core file; keep those in `templates/` or a `sport-packs/` subfolder so the core skill stays sport-agnostic.

## License

MIT. Use it, fork it, change it.
