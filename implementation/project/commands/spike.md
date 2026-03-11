---
description: Comprehensive research and investigation. Explores codebase, external sources, best practices, and evaluates approaches. No code written. Ends with a findings summary.
argument-hint: [research-question]
allowed-tools: Read, Grep, Glob, Bash, WebSearch
model: sonnet
---

Investigate the question provided in the arguments (e.g. `/spike how does care plan generation work` or `/spike should we use React Query for data fetching`).

Do not write any code. Do not make any changes. Only read, explore, and research.

## Step 1 — Define the question

Restate the question clearly in one sentence. If it's vague, break it into sub-questions.

**If this is about evaluating an AI-suggested approach:**
- What problem is it trying to solve?
- What are the alternatives?
- What are the trade-offs?

## Step 2 — Explore codebase

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

## Step 4 — Research external sources

**Search for how other engineers solve similar problems:**

1. **Web search for articles and best practices:**
   - Search for: `<topic> best practices`, `<topic> patterns`, `<topic> implementation guide`
   - Look for authoritative sources (official docs, well-known blogs, Stack Overflow)
   - Find real-world examples and case studies

2. **GitHub search for implementations:**
   ```bash
   gh search code "<keyword>" --language=TypeScript --limit=10
   gh search repos "<keyword>" --language=TypeScript --limit=10
   ```
   - Find similar implementations in popular repos
   - See how others structure their code
   - Check for common patterns or anti-patterns

3. **MCP servers for official documentation:**
   - **AWS/Terraform:** Use `aws-docs` and `terraform` MCPs for infrastructure questions
   - **React Native/Expo:** Use `context7` MCP for framework-specific questions
   - **Strapi:** Use `strapi-docs` MCP for CMS-related questions
   - **PostgreSQL:** Use `postgres` MCP for database questions
   - **Express/Node:** Use `context7` MCP for backend questions

4. **Compare approaches:**
   - What are the pros and cons of each approach?
   - What are the performance implications?
   - What are the maintenance costs?
   - What are the security considerations?
   - What fits your current architecture?

## Step 5 — Evaluate for your context

**If evaluating an AI-suggested approach:**
- Does it align with your current patterns?
- Does it introduce unnecessary complexity?
- Are there simpler alternatives?
- What are the migration costs?
- What are the long-term implications?

**Consider:**
- Your tech stack (React Native, Expo, Express, Terraform, AWS)
- Your team's expertise
- Your project's constraints (performance, scale, budget)
- Your existing patterns and conventions

## Step 6 — Document findings

Output a structured summary:

```
## Question
<the original question>

## What exists (in codebase)
<what the codebase already does, with file:line references>

## External research

### Articles & Best Practices
- [Title](URL) — <key insight>
- [Title](URL) — <key insight>

### GitHub Examples
- [Repo](URL) — <how they implemented it>
- [Repo](URL) — <alternative approach>

### Official Documentation
- [AWS Docs](URL) — <relevant section>
- [Terraform Docs](URL) — <relevant section>
- [Framework Docs](URL) — <relevant section>

### Common Patterns Found
- Pattern A: <description> — used by X% of repos
- Pattern B: <description> — used by Y% of repos

## Evaluation

### Approach 1: <name>
**Pros:**
- <benefit>

**Cons:**
- <drawback>

**Fits our context:** Yes/No — <reasoning>

### Approach 2: <name>
**Pros:**
- <benefit>

**Cons:**
- <drawback>

**Fits our context:** Yes/No — <reasoning>

## What's missing or unclear
<gaps, inconsistencies, or things that need further investigation>

## Key files (in codebase)
- path/to/file.ts — <why it's relevant>

## Recommendation
<recommended approach with reasoning, or the next questions that need answering>

**Why this makes sense for us:**
- <reason 1>
- <reason 2>
- <reason 3>
```

**Always include citations and references** for external sources.

No implementation. If a plan is needed after the spike, use `/start-ticket` or `/plan`.
