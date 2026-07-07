---
name: fable-memory
description: Use when working in a project that has CLAUDE.md or memory files, before choosing an implementation approach or building custom tooling, when asked to remember something, when a session runs past ~30 turns, or when reviewing/analyzing material a prior version of your own output may already cover.
---

# Fable-Style Memory Discipline

## Overview

Memory is only useful if consulted at decision points, not just written. Fable 5 recovered a 120-turn session by re-consulting a stored user preference mid-task and switching approach ("critical recall" at turn 119, Fable SC p.42-43); Opus 4.8's documented failure is the opposite — it wrote a correct rule to memory, then violated that same rule repeatedly in one session (Opus SC p.32-34).

## Quick reference

| Moment | Action |
|---|---|
| Before picking an approach or building tooling | Grep CLAUDE.md + memory files; quote the hit or say "no relevant memory found" |
| Phase transition in a 30+-turn session | Re-read memory files |
| Before irreversible or user-visible actions | Re-check memory for constraints |
| Writing a new rule to memory | Also add it to the active todo list |
| Reviewing something you may have reviewed before | Independent draft FIRST, diff against prior output after |
| Saving a fact | One file per fact; update duplicates; index in MEMORY.md |

## Consult before acting

Before choosing an implementation approach, building any custom tooling, or touching the user's live environment, grep or read CLAUDE.md and all memory-directory files for relevant preferences and prior lessons. Quote the matching line explicitly — or state "no relevant memory found" — before proceeding. If memory records a preferred tool for a task class (e.g. Playwright for browser capture), use it instead of inventing a new mechanism (Fable SC p.39, 42-43). Exception: on review, audit, or analysis tasks, this consultation is for preferences and constraints only — if a memory hit is your own prior review or analysis of the material under review, do not read past recognizing what it is; flag it and set it aside until your independent draft is complete (Anti-anchoring below takes precedence).

In sessions longer than ~30 turns, re-read memory files at each phase transition and before any irreversible action: storage is not retrieval, and rules decay under load (Opus SC p.32-34). When you write a new rule to memory, immediately restate it and add it as an explicit item on your active todo list so it is enforced on every subsequent step, not just recorded.

## Anti-anchoring on your own prior outputs

When reviewing, auditing, or analyzing material for which a prior version of your own output may exist — in memory files, prior sessions, searchable documents, or earlier in context — do NOT search for or read that prior output until your independent analysis is complete. Anthropic documented that Opus found its own earlier review and "defaulted to producing a very similar review rather than working from scratch"; an explicit instruction fixed it (Opus SC p.86). Work from primary sources; only after a complete independent draft may you diff against the prior version to catch omissions. If prior output surfaces accidentally, flag it and state that you are setting it aside.

## Writing memory

Follow the Fable memory protocol (Fable session guidance): one fact per file, with frontmatter — `name` (kebab-case slug), `description` (one line, used for recall relevance), and `type` (user | feedback | project | reference). After writing, add a one-line pointer to MEMORY.md, the index loaded each session; never put memory content in the index itself. Before saving, check whether an existing file already covers the fact — update it rather than duplicating; delete memories that turn out to be wrong. Do not save what the repo already records (code structure, git history, CLAUDE.md content) or what only matters to the current conversation.

Related: verification of claims about what memory said belongs to fable-verification; keeping session state across compaction belongs to fable-long-tasks.
