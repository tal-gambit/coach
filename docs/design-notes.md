# Design notes

Reasoning behind the choices in this skill, for anyone extending or forking it.

## Why a procedure, not a personality

The early temptation in writing a "coach" skill is to describe a personality: direct, evidence-based, warm but honest. That degrades badly over a long relationship — tone drifts, and there's nothing to check a given response against. Instead, the skill's "Core stance" and "Reviewing a session" sections are written as a checkable sequence: retrieve data before commenting, compare against history, check standing constraints, name deviations, log only confirmed sessions. Each of those is either done or not done in a given turn. That's what makes the skill enforceable rather than aspirational.

## Why the standing summary and the time series are separate files

Two different reads happen constantly in a coaching relationship, and they want different shapes:

- "What's the situation?" — read once at the start of a session, wants to be short: goal, phase, constraints, milestones.
- "What actually happened last time / how does this compare?" — wants full detail on specific past sessions, searched rather than read wholesale.

Cramming both into one file means either the summary bloats until it's slow to read, or the log gets thin enough to lose the detail that makes comparisons useful. Splitting them lets the summary stay fast and the log stay rich.

## Why "standing rules" live in the area file, not the log

Rules that get established mid-training — "don't stack grip-intensive sessions two days running," "a short-but-present warm-up is fine, a zero warm-up isn't" — need to be checked *every session*, not rediscovered by reading the whole log each time. Putting them in the always-read summary file means they're cheap to check. If they only existed buried in a workout log entry from three weeks ago, the skill would either need to re-read the entire log every time (expensive, and easy to skip) or would silently drop the rule.

## Why "log only what's confirmed" is explicit

Without this, there's a natural failure mode where a planned session and a completed one blur together — Claude writes up what was *supposed* to happen as if it happened, especially when the athlete's message is ambiguous ("doing the tempo run today" could mean "about to" or "just did"). The log's value depends entirely on it reflecting reality; a log that's sometimes aspirational is worse than no log, because it's trusted the same amount either way.

## Why the correction protocol is explicit

Long coaching relationships involve Claude being wrong sometimes — misreading data, getting a date wrong, drawing a conclusion from incomplete information that later turns out incorrect. The failure mode isn't the error itself, it's the overcorrection afterward: excessive hedging, apologizing at length, or losing confidence in unrelated later judgments. Stating explicitly that the right move is "acknowledge, re-derive, move on" heads that off.

## What's deliberately left sport-agnostic

The core file has no exercise prescriptions, no sport-specific terminology beyond generic examples, and no assumption about what "a session" looks like. That's intentional — the procedure is the same whether the goal is an OCR race, a strength block, a marathon, or a return from injury. Sport-specific knowledge (how to structure a tempo run, what a deload week means, terrain-specific race prep) belongs in the conversation itself or in a `sport-packs/` extension, not baked into the core skill.

## Known limitations

- **No install mechanism guarantees this gets read.** Depending on the Claude surface, the user may need to explicitly reference it or add it to project instructions. This skill can't fix distribution/discovery — that's a platform-level concern.
- **Memory is per-account.** This skill defines the shape of what gets remembered; it can't transplant one person's training history into another's fresh account. Each install starts from the Setup section.
- **Data connector availability varies.** The skill is written to degrade gracefully (ask for manual reports) when no health/calendar connector exists, but the coaching quality is genuinely lower without one — that's a real tradeoff, not a solved problem, and the skill says so rather than pretending otherwise.
