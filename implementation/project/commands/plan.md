---
description: Create a phased implementation plan for a feature or change. Uses planner agent.
argument-hint: [feature-description]
allowed-tools: Read, Bash, Grep, Glob
model: sonnet
---

Create a phased implementation plan for the feature or change described.

## Step 1 — Understand Requirements

Clarify:
- What is the goal?
- What are the constraints?
- What are the dependencies?
- What are the risks?

## Step 2 — Delegate to Planner Agent

Invoke the `planner` agent to:
- Break down the work into phases
- Identify dependencies between phases
- Estimate complexity
- Identify risks and mitigation strategies

## Step 3 — Review Plan

Present the plan with:
- Phase breakdown
- File paths and changes per phase
- Testing strategy per phase
- Dependencies and risks

## Step 4 — Wait for Confirmation

Wait for user approval before proceeding with implementation.
