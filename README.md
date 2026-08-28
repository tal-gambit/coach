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

## Installation

Skill installation surfaces differ across Claude products and change over time — check [docs.claude.com](https://docs.claude.com) for the current method for your platform (claude.ai Projects, Claude Code, Claude Cowork, etc.). In general, this means either:

- Adding `SKILL.md` to a Project's knowledge/instructions, or
- Placing it wherever your Claude environment loads custom skills from, so it's available when you start a coaching conversation.

Once installed, just start talking to Claude about your training goal — the skill's Setup section handles onboarding.

## Recommended connections

- **A health/fitness data source** (Apple Health, or whatever your platform supports) — without this, Claude is coaching from your self-reports alone, which the skill will tell you plainly rather than pretend otherwise.
- **A calendar** (Google Calendar or equivalent) — for scheduling sessions with full instructions in the event body, not just a title.

Neither is required to start. The skill degrades gracefully and will ask for manual reporting or screenshots if no data connector is available.

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
