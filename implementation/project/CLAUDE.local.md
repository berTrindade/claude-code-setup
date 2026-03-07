# Project — Personal Project Notes

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

[Project description]

## Commands

All commands must be run from within the respective package directory.

## Architecture

[Architecture overview]

## Code Conventions

- **TypeScript** across all packages, strict mode
- **ESLint + Prettier**: semi: false, singleQuote: true, tabWidth: 2, printWidth: 100
- **ESM only**: all Node packages use `"type": "module"`
- Environment variables follow `.env.example` templates
- Branch naming: `feature/`, `fix/`, `refactor/` prefixes

## Infrastructure

**Patterns and Best Practices:**
- See `.claude/rules/infrastructure/terraform-patterns.md` for comprehensive AWS/Terraform patterns
- See `.claude/rules/infrastructure/secrets-management.md` for Flow A (ephemeral) and Flow B (external) secrets patterns

**Terraform Patterns:**
- Naming: `{project}-{environment}-{component}`
- Tagging: Standard tags via tagging module
- Secrets: Flow A (ephemeral passwords) for DB, Flow B (external seeding) for API keys
- State: S3 backend with lockfile
- Safety: Always check existing AWS resources and verify state backend before changes
