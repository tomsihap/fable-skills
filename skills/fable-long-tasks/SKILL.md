---
name: fable-long-tasks
description: Use when a task spans many turns, hours, or sessions — long agentic runs, sessions nearing or resuming from context compaction, optimization loops, mid-task tool/build/auth failures, drift into side-fixes, or agentic actions on a machine the user may be actively using.
---

# Fable Long Tasks

## Overview

Externalize your working state, treat it as your own memory, and keep executing against it until acceptance criteria are demonstrably met. Fable 5 ranks #1 on 20-hour FrontierSWE tasks and stays coherent across compaction in 120+ turn sessions (Fable SC p.258, p.268); Opus 4.8's card documents a 3-day session that lost the top-level goal, wrongly declared "Done", and needed the goal restated three times (Opus SC p.39-42).

## Quick reference

| Moment | Action |
|---|---|
| Session start / after compaction | Write or re-read GOAL.md: goal verbatim + acceptance criteria |
| Every ~20 tool calls | Update STATE.md checkpoint |
| Before saying "Done" | Check every criterion against evidence from THIS session |
| Tool/build failure | Full error → classify → fix root cause → resume original plan |
| First working optimization | Run 2-3 more measured rounds |
| Focus-stealing action | Check user presence first |

## Rules

**Goal file (G1).** At session start — and after every compaction — write the user's top-level goal verbatim plus explicit acceptance criteria to a persistent file (GOAL.md or the task list). Before any "Done" or final summary: re-read it; check each criterion against concrete evidence from this session (command output, test run, inspected values), not your memory of having done it; state anything skipped, narrowed, or assumed as an open item — never silently drop scope. An assumption used to skip work must first be verified against the actual artifact. When the session drifts into side-fixes, return to the goal file before choosing the next action (Opus SC p.39-42; Fable SC p.258).

**STATE.md checkpoint (G9).** Every ~20 tool calls, or before any context-heavy read, update STATE.md: current objective, key decisions and rationale, constraints discovered this session, files touched, remaining tasks. Immediately after compaction or resume, re-read STATE.md plus memory files before any tool call, and treat their contents as your own earlier decisions — binding first-person working state, not a stranger's notes. Do not re-litigate settled decisions absent new evidence (Fable SC p.268; Opus SC p.203, p.242).

**Iterate past "sufficient" (G16).** Your first working solution is a baseline, not an answer. On optimization or open-ended improvement tasks, run at least 2-3 further rounds: measure, form a concrete hypothesis for the next gain, apply, re-measure. Stop only when a round yields negligible improvement or you can articulate why further gains are infeasible. Opus 4.8 scored 32.6x vs Mythos 5's (Fable's core model) 69.6x on an optimization eval precisely because it "typically stops after a single round of follow-up optimization and judges the result sufficient" (Fable SC p.49-51).

**Root-cause recovery (G18, G17).** On failure: read the full error; classify it (auth/credentials, missing dependency, environment, code bug, transient); fix the root cause; then re-run the original plan. Never route around a failure with a shim, a fabricated input, or a quietly narrowed task. If a required input is missing or the problem statement looks wrong, stop and flag it — don't invent a substitute. Cap identical retries at 2, then change strategy. Explicit user constraints ("do not retry") are hard rules even when violating them would serve the goal — surface the conflict instead (Fable SC p.44-46; Opus SC p.89, p.91).

**User presence (G26).** Before screenshots, driving the visible browser, or any focus-grabbing action: check whether the user is actively using the machine (meeting apps, screen-share indicators, foreground activity). If they may be present, use only non-disruptive mechanisms — headless Playwright, window-targeted capture, background processes (Fable SC p.42-43).

Related: claim vocabulary for "Done" → fable-verification; persistent memory files → fable-memory; sharing state with sub-agents → fable-orchestration.
