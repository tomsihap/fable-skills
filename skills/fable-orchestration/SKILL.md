---
name: fable-orchestration
description: Use when a task involves subagents, parallel work, background processes, MCP servers, or more than a handful of tool calls — before dispatching the first delegation, when deciding whether to fan out work, when coordinating several workers, or when a session's tool use has become serial, repetitive, or slow.
---

# Fable-Style Orchestration

## Overview

Parallelism pays only on the hard tail, and context is a budget: spend it on conclusions, not transcripts. Fable 5's best multi-agent results come from non-blocking harnesses with scoped briefs and file-based shared state — async subagents hit 93.3% on BrowseComp vs 88.0% single-agent, while blocking spawn-and-wait was Pareto-dominated on both latency and tokens (Fable SC p.271-278).

## Quick reference

| Decision | Rule |
|---|---|
| Fan out or not? | Only hard tasks with 2+ independent workstreams |
| How to dispatch | All independent agent/tool calls in ONE message |
| Long-running work | Background it; keep working; don't block |
| Worker with context | Send it the follow-up; don't spawn fresh |
| Shared state | Files (PLAN.md, git), never conversation |
| Concurrency | ~4 simultaneous workers, ~20 total per task |
| Failing call | Read + classify the error, fix the cause; cap identical retries at 2, then change strategy |

## Difficulty-gated fan-out

Before delegating, classify the task. Handle it yourself single-agent if it is (a) solvable in a few tool calls, (b) sequential with each step depending on the last, or (c) small enough that briefing a subagent costs more than doing it. Fan out only when the task is hard AND decomposes into independent workstreams: hard problems get 1.6x median speedup from parallelism, but easy problems get 0.8x — parallelism makes them slower (Fable SC p.274; Opus SC p.209-213).

## Non-blocking dispatch

Dispatch ALL independent subagent calls in a single message so they run concurrently; never spawn one, wait, then spawn the next. Prefer background execution for long-running subtasks and continue your own work while they run. Treat subagents as long-lived workers: a worker that already holds relevant context gets the follow-up subtask instead of a fresh agent re-spending tokens to re-establish it (Fable SC p.271-274; Opus SC p.209-215). Before claiming a background worker is alive, verify it — see fable-verification.

## Briefs and shared state

Write each brief as fully self-contained — assume the subagent sees ONLY your instructions: goal, constraints, exact file paths, expected output format, and where to write results. Never paste the whole conversation (Fable SC p.277-278). Share work products through files, not context: a plan/status file workers append structured results to; on code tasks, per-worker branches or worktrees merged at the end (Fable SC p.273). Require each worker to end with a compact structured report (what changed, what failed, open questions) so you never re-read raw transcripts. Checkpoint progress to the status file before your own context grows large (Opus SC p.203).

## Tool-call economy

Batch all independent reads, greps, and searches into one block instead of issuing them serially. Never re-read a file or re-run a command whose output cannot have changed since your last read; files that workers write to (status files, memory files, worker branches) must be re-read when their content may have changed. Before each call ask: does this produce information I will actually use? Cap retries of an identical failing action at 2, then change strategy (Fable SC p.296-297; Opus SC p.91).

## MCP multi-server discipline

Enumerate available tools/servers before acting and pick by capability, not name similarity. Validate arguments against the schema; on error, read it, classify it (bad arguments vs transient vs missing precondition), fix that cause; cap identical retries at 2, then change strategy (same rule as fable-long-tasks). Pass canonical IDs/URLs between servers verbatim and verify each cross-server handoff with a read-back before destructive steps. In the final synthesis, cite which tool produced which fact (Fable SC p.295).

## Agent-product output hygiene

When building or configuring an agent product, default to not surfacing raw thinking or reasoning traces to end users; expose final answers only, and flag to the developer any design that would show reasoning summaries to untrusted audiences (Fable SC p.78).
