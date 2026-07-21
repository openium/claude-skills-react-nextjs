# Claude Skills React/Next.js

This repository contains Claude Code skills for React, TypeScript, and Next.js.

Skills are installed in the `.claude/skills/` folder. Each skill has a
`SKILL.md` file that describes its usage, rules, and conventions.

To use a skill in a project, copy its folder into `.claude/skills/` at the root
of the target project. To install all skills, copy the entire contents of this
repository into the target project's `.claude/skills/` folder.

Detailed conventions will be added progressively.

## Available Skills

### Code Quality

| Skill | Description |
|-------|-------------|
| [`/review`](.claude/skills/review/SKILL.md) | React/Next.js code review: bugs, typing, Server/Client Components, security, accessibility, performance, tests. |
| [`/refactor`](.claude/skills/refactor/SKILL.md) | Safe refactoring of components, hooks, and TypeScript code without observable behavior changes. |

### UI, Data, And Tests

| Skill | Description |
|-------|-------------|
| [`/component`](.claude/skills/component/SKILL.md) | Creates or modifies components that comply with the existing design system. |
| [`/data-fetching`](.claude/skills/data-fetching/SKILL.md) | Loading, cache, revalidation, pagination, and error strategy on the server or client side. |
| [`/forms`](.claude/skills/forms/SKILL.md) | React/Next.js forms: validation, submission, errors, accessibility, and tests. |
| [`/test`](.claude/skills/test/SKILL.md) | Unit and component tests with Jest/Vitest and React Testing Library according to the project. |

### Security, Performance, And Observability

| Skill | Description |
|-------|-------------|
| [`/accessibility`](.claude/skills/accessibility/SKILL.md) | Accessibility audit: semantics, keyboard, focus, forms, contrast, and checks. |
| [`/security`](.claude/skills/security/SKILL.md) | React/Next.js security audit: XSS, auth, IDOR, CSRF, secrets, API routes, and dependencies. |
| [`/performance`](.claude/skills/performance/SKILL.md) | Performance audit: rendering, hydration, bundle, cache, images, fonts, and Core Web Vitals. |
| [`/observability`](.claude/skills/observability/SKILL.md) | Logs, errors, traces, metrics, Sentry/APM, alerts, and sensitive data protection. |

### Planning And Onboarding

| Skill | Description |
|-------|-------------|
| [`/plan`](.claude/skills/plan/SKILL.md) | React/Next.js implementation plan before coding: steps, files, tests, validations, risks. |
| [`/debug`](.claude/skills/debug/SKILL.md) | Reproducible diagnosis of frontend or Next.js errors without automatic fixes. |
| [`/upgrade`](.claude/skills/upgrade/SKILL.md) | Upgrade plan for Node.js, React, Next.js, TypeScript, and frontend dependencies. |
| [`/onboard`](.claude/skills/onboard/SKILL.md) | Technical onboarding guide for an existing React/Next.js project. |

## Invocation Examples

| Need | Skill | Example |
|------|-------|---------|
| Review a diff before commit | `/review` | `/review analyze the staged diff before commit` |
| Diagnose an error | `/debug` | `/debug analyze this Next.js hydration error` |
| Simplify a component | `/refactor` | `/refactor simplify components/ProductCard.tsx without changing behavior` |
| Create a component | `/component` | `/component create a Modal component that matches the design system` |
| Fix data loading | `/data-fetching` | `/data-fetching fix revalidation for the product list` |
| Add a form | `/forms` | `/forms create the contact form with server validation` |
| Add tests | `/test` | `/test add tests for the ProductCard component` |
| Audit accessibility | `/accessibility` | `/accessibility audit the checkout form` |
| Audit security | `/security` | `/security audit the modified API routes` |
| Diagnose slowness | `/performance` | `/performance analyze the dashboard page bundle` |
| Improve logs | `/observability` | `/observability propose useful logs around the payment Server Action` |
| Plan a feature or bugfix | `/plan` | `/plan prepare the implementation of <feature-or-bugfix>` |
| Prepare an upgrade | `/upgrade` | `/upgrade prepare the move to Next.js <version>` |
| Document a project | `/onboard` | `/onboard generate a guide for a new developer` |

## Structure Convention

Each skill is a folder in `.claude/skills/<name>/` with a required `SKILL.md`
file.

```text
.claude/
└── skills/
    └── skill-name/
        └── SKILL.md
```

The folder name and the `name` field must be identical.

```yaml
---
name: skill-name
description: "Short and precise description of when to use this skill."
---
```

## Project Template

A `CLAUDE.md` template is available in `templates/CLAUDE.md`. It is used as a
base for creating project instructions tailored to an existing React,
TypeScript, and Next.js application.

Short prompt:

```text
From the templates/CLAUDE.md template, generate a CLAUDE.md tailored to this project.
First inspect the stack, lockfile, Next.js router, scripts, tests, and existing
conventions. Keep the file concise and do not read `.env` or sensitive
environment files.
```
