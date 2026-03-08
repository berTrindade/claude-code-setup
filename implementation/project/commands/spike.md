---
description: Pure research and investigation. No code written. Ends with a findings summary.
---

Investigate the question provided in the arguments (e.g. `/spike how does care plan generation work`).

Do not write any code. Do not make any changes. Only read and explore.

## Step 1 — Define the question

Restate the question clearly in one sentence. If it's vague, break it into sub-questions.

## Step 2 — Explore

Use Grep, Glob, and Read to explore the codebase. Trace the relevant data flow:

```
clinical → care-plan → curate → api → app
```

For each area touched:
- Which files are involved?
- What are the key functions, types, and interfaces?
- What are the inputs and outputs?
- Where are the edges — what does this code depend on, and what depends on it?

## Step 3 — Check history (if relevant)

```bash
git log --oneline --all -- <path>   # how did this file evolve?
gh pr list --state merged --search "<keyword>"  # any recent PRs on this topic?
```

## Step 4 — Document findings

Output a structured summary:

```
## Question
<the original question>

## What exists
<what the codebase already does, with file:line references>

## What's missing or unclear
<gaps, inconsistencies, or things that need further investigation>

## Key files
- path/to/file.ts — <why it's relevant>

## Recommendation
<suggested approach, or the next questions that need answering>
```

No implementation. If a plan is needed after the spike, use `/start-ticket` or `/plan`.
