---
name: fable-effort
description: Use when starting a hard reasoning problem — math, multi-step planning, algorithm design, debugging with non-obvious cause — when choosing a reasoning-effort setting, or when the task includes user-provided data, examples, or reference corpora that could be taken at face value.
---

# Fable Effort

## Overview

Spend reasoning where difficulty lives, and treat provided data as a claim to assess, not a given. Fable/Mythos 5's scores scale monotonically with reasoning effort, and it stays stable when handed a potentially misleading corpus where Opus-class models degrade (Fable SC p.261-262, p.33).

## Quick reference

| Situation | Default |
|---|---|
| Hard reasoning, math, multi-step planning | Reason step-by-step before answering; high effort |
| Picking an effort setting for Opus 4.8 | high/xhigh default (empirical optimum is medium–xhigh by task); spot-check — more is not always better |
| User-supplied corpus/examples/references | Assess relevance and reliability first, out loud |

## Rules

**Think before answering (G37).** For non-trivial problems, reason thoroughly and step-by-step before committing to an answer; do not shortcut to a quick reply on tasks that reward deliberation. Where the harness exposes extended thinking or a reasoning-effort setting, prefer high effort for hard reasoning, math, and multi-step agentic work (Fable SC p.261-262).

**Don't blindly max effort (D1 advisory).** Opus 4.8's effort scaling is non-monotonic: it peaked at *medium* effort on FrontierCode Diamond and dropped from xhigh to max on FrontierCode Main (Fable SC p.255-258). Opus's empirical optimum ranges medium–xhigh depending on task; default to high/xhigh but be willing to drop to medium where evidence supports it — on a repeated task class, check which setting actually wins instead of assuming more is better.

**Assess provided data (G36).** Before incorporating any supplied dataset, corpus, examples, or reference material: state explicitly whether it is relevant and reliable for the actual task. Down-weight or ignore material that could mislead; prefer a principled reasoning strategy over pattern-matching to whatever data is present. Opus-class models "perform substantially worse, with lower means and higher variance" when given an unlabeled misleading corpus; Mythos 5 holds stable (Fable SC p.33; Opus SC p.27-28).

Related: verifying the result once produced → fable-verification; iterating past the first sufficient result → fable-long-tasks.
