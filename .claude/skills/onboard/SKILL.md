---
name: onboard
description: "Produces a technical onboarding guide from an existing React/Next.js project. Analyzes architecture, versions, installation, commands, routing, components, data, authentication, tests, conventions, CI/CD, observability, and known pitfalls without reading sensitive environment files."
---

# React / Next.js Technical Onboarding

## Scope

By default, produce a guide for the whole project. If the user specifies a
module, folder, or goal, limit onboarding to that scope.

Do not modify the repository unless explicitly requested.

## Sources To Inspect

Read when present:

- `README*`, `docs/`
- `package.json`, lockfile
- `.nvmrc`, `.node-version`, `Dockerfile`, `docker-compose*`
- `next.config.*`, `tsconfig.json`, lint, Prettier
- `app/`, `pages/`, `src/`, `components/`, `lib/`, `services/`, `hooks/`
- `public/`, styles, design system
- tests, Storybook, Playwright/Cypress
- `.github/workflows/`, `.gitlab-ci.yml`, deployment config
- `.env.example` or environment documentation if non-sensitive

Never read or copy `.env` or sensitive environment files.

## Rules

- Document only what is verifiable in the repo.
- Distinguish verified facts from points to confirm.
- Cite consulted source files when useful.
- Do not invent commands: use existing `package.json`, Makefile, or CI scripts.
- If README, Docker, scripts, and CI contradict each other, report the mismatch.
- Document environment variables by name and role only if the source is non-sensitive.
- If a secret appears to be committed, report it without copying it.

## Guide Sections

### Overview

Project purpose, application type, main domains, external services.

### Stack And Versions

Node.js, package manager, React, Next.js, TypeScript, styles, tests, tools.

### Installation And Commands

Prerequisites, installation, local startup, build, tests, lint, Storybook if present.

### Architecture

Folder structure, conventions, UI/data/domain separation, providers.

### Routing

App Router, Pages Router, or hybrid, main routes, API routes, middleware.

### Components And Design System

Component organization, UI primitives, styles, responsive behavior, accessibility.

### Data And Authentication

Data fetching, cache, mutations, auth/session, API clients, sensitive data.

### Tests And Quality

Jest/Vitest, Testing Library, E2E, lint, typecheck, test conventions.

### CI/CD And Deployment

Pipelines, variables, build, preview, production, rollback if verifiable.

### Observability

Logs, Sentry/APM, analytics, source maps, alerts if present.

### Known Pitfalls

TODO/FIXME, contradictory documentation, legacy areas, test limitations.

## Output Format

Produce a factual and concise Markdown document with:

- useful commands in `sh` blocks;
- references to consulted files when relevant;
- **To Confirm** section;
- **Documentation Gaps Or Mismatches** section if necessary.

Do not include secrets or sensitive environment file content.
