# Fable Skills — emulating Claude Fable 5 on Claude Opus 4.8

A pack of 10 Claude Code skills that brings **Claude Opus 4.8**'s behavior as close to **Claude Fable 5**'s as prompt-level instructions allow. The hard limits are documented in [`DIVERGENCES.md`](DIVERGENCES.md).

## Quick start

```bash
git clone https://github.com/tomsihap/fable-skills.git
cd fable-skills

# Personal skills (available in all your Claude Code sessions):
cp -r skills/* ~/.claude/skills/

# — or — project skills (a single repo):
cp -r skills/* /path/to/your/project/.claude/skills/
```

Then, in a Claude Code session running Opus 4.8, type **`/fable-mode`** at the start of the session (and again after any context compaction). It loads the routing index and the always-on core rules; the nine other skills load on demand via their triggers, or manually (`/fable-verification`, `/fable-concision`, ...).

To make it automatic, add one line to your `CLAUDE.md` (global or project):

```markdown
At the start of every session and after each context compaction, read the fable-mode skill and apply its routing.
```

## Method

1. Both system cards (319 + 246 pages) were read **in full** by 15 parallel reader agents, producing 580 page-cited findings.
2. 7 comparator agents consolidated those findings into **46 behavioral gaps** (where Fable behaves better or differently, and where a prompt can act) and **43 divergences** (where nothing can be done at the prompt level) — full matrix in [`docs/gap-analysis.md`](docs/gap-analysis.md).
3. The 46 gaps were grouped into 10 domain-scoped skills — the format Anthropic itself uses for its Fable-class agents ("per-portal skill files rather than task-specific system-prompt text", Fable SC p.301).
4. Secondary source: this repo was written *by* Fable 5, which folded in its own session-level behavioral guidance (marked "Fable session guidance" in the skills) wherever it reinforces a documented gap.

> **Note:** the system card PDFs are not redistributed in this repo (copyrighted Anthropic documents). Get them from Anthropic — see the [Fable 5 & Mythos 5 announcement](https://www.anthropic.com/news/claude-fable-5-mythos-5) and Anthropic's published system cards — and drop them into `docs/` if you want to check page citations or reproduce the analysis.

## The 10 skills

| Skill | Trigger | Gaps covered |
|---|---|---|
| `fable-mode` | Session start, after compaction — index + always-on core | G19 + routing |
| `fable-verification` | Before any "done / verified / running" claim | G0, G2, G10, G15, G38 |
| `fable-long-tasks` | Long sessions, mid-task failures, compaction | G1, G9, G16, G18, G26 |
| `fable-orchestration` | Subagents, parallelism, MCP, background jobs | G3, G4, G17, G20, G22, G28, G39 |
| `fable-concision` | Any user-facing reply | G5, G25, G40, G41, anti-G42 |
| `fable-honesty` | User pushback, contested topics, self-reports | G11, G24, G29, G30, G43*, G44, G45 |
| `fable-refusals` | Tempted to refuse; dual-use requests; social pressure | G6, G14, G23, G31, G32, G33, G34 |
| `fable-sensitive-topics` | Mental health, eating disorders, substances | G7, G12, G13, G35 |
| `fable-memory` | Choosing an approach, building tooling, review tasks | G8, G21, G27 |
| `fable-effort` | Calibrating reasoning effort, dubious provided data | G36, G37, D1 advisory |

\* of G43, only the exception rule (never guess citations/CLI syntax) is shipped — see "What this pack deliberately does not emulate".

## Recommended harness settings (outside the skills)

These levers are configuration, not prompting:

- **Reasoning effort: `medium` to `xhigh` depending on the task (a reasonable default: `high`/`xhigh`), never blindly `max`.** Opus 4.8 plateaus and then regresses past its optimum (peak at `medium` on FrontierCode Diamond; 34.3% → 31.3% from xhigh to max on Main — Fable SC p.255-258). Test empirically on your workload.
- **Extended thinking on** for hard tasks (Opus's scaling stays positive up to its peak).
- **Do not expose reasoning traces** to end users of your agent products (Fable SC p.78).
- The Claude Code harness (Task tool, background tasks, file-based memory) already provides the infrastructure that `fable-orchestration` and `fable-long-tasks` exploit.

## What this pack deliberately does not emulate

Fidelity ≠ mimicry: on several documented axes, Fable is *worse* than Opus 4.8, and the pack keeps Opus's advantage — better factual honesty via abstention (D19), lower agentic destructiveness (D4), no "context anxiety" (D3), no grader-incentivized verbal tics (G42). Details in section 7 of [`DIVERGENCES.md`](DIVERGENCES.md).

## What stays out of reach

Raw capability (code, math, vision, 1M context), injection/jailbreak robustness, the classifier stack, reasoning texture: see [`DIVERGENCES.md`](DIVERGENCES.md). In short: **this pack transfers Fable's discipline, not its brain.**

## Traceability and testing

- Every rule drawn from the system cards cites the pages that ground it; rules drawn from Fable's session guidance are marked "Fable session guidance" (no page numbers).
- Full gap/divergence matrix: [`docs/gap-analysis.md`](docs/gap-analysis.md).
- Test status: multi-agent adversarial review (12 reviewers — format, page-level grounding, harmful drift, cross-skill conflicts; 49 issues found and fixed), plus blind A/B tests on real Opus 4.8 (3 scenarios × 2 arms × 3 repetitions, independent rubric-scoring judges). Key result: `fable-concision` moves Opus from 7.3/10 to 9.7/10 with replies 3-6x shorter; `fable-verification` and `fable-honesty` hit the 10/10 ceiling on short scenarios (the failures they target are long-session phenomena, not reproducible in a micro-test). Details: [`docs/ab-tests.md`](docs/ab-tests.md).
