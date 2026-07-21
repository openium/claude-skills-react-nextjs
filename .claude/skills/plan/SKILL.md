---
name: plan
description: "Plans the implementation of a feature, bugfix, refactor, frontend migration, or upgrade in a React, TypeScript, and Next.js project. Analyzes the stack, router, components, routes, hooks, services, tests, cache/rendering/hydration/accessibility/bundle impacts, then produces an ordered plan without modifying code."
---

# React / Next.js Implementation Planning

## Scope

Determine whether the user is asking for a plan for:

- UI feature or user journey
- React, TypeScript, Next.js, rendering, or hydration bugfix
- Targeted refactor of components, hooks, services, or routes
- Limited frontend migration
- React, Next.js, TypeScript, tooling, or dependency upgrade
- Performance, accessibility, bundle, or data fetching fix
- Addition or evolution of unit, component, or E2E tests

If the user asks only for a plan, never modify the code.

If the scope is ambiguous, ask only blocking questions. For non-blocking points,
state explicit assumptions.

## Goal

Produce an executable, ordered plan tailored to the project.

The plan must:

- Respect the conventions and versions actually detected.
- Preserve the existing App Router, Pages Router, or hybrid architecture.
- Separate components, routes, hooks, services, styles, tests, and configuration.
- Identify impacts on cache, rendering, hydration, accessibility, and bundle.
- Keep the future diff minimal.
- Avoid migrations, dependencies, and refactors that were not requested.
- Provide useful validations without running destructive actions.

## Current State To Inspect

Inspect according to the subject:

- `package.json`: scripts, React/Next.js/TypeScript dependencies, tooling.
- Lockfile: `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`, or `bun.lockb`.
- TypeScript: `tsconfig.json` and extended configurations.
- Next.js: `next.config.js`, `next.config.mjs`, `next.config.ts`, middleware.
- Lint/format: `eslint.config.*`, `.eslintrc*`, Prettier, Stylelint.
- Tests: Vitest, Jest, Testing Library, Storybook, Playwright, Cypress.
- Router: presence of `app/`, `pages/`, `src/app/`, `src/pages/`.
- Routes: layouts, pages, route handlers, API routes, middleware, dynamic routes.
- UI: components, styles, design system, existing accessibility.
- Logic: hooks, services, API clients, stores, providers, validators.
- Data fetching: `fetch`, Server Components, server actions, SWR, TanStack Query, GraphQL.
- Observability: Sentry, Datadog, OpenTelemetry, analytics, platform logs.

Do not read or display `.env` or sensitive environment files.

## Next.js Analysis

Detect and state explicitly:

- App Router: `app/`, layouts, Server Components, Client Components, route handlers.
- Pages Router: `pages/`, `getServerSideProps`, `getStaticProps`, API routes.
- Hybrid architecture: coexistence of `app/` and `pages/`, responsibilities of each area.
- Server/client boundaries: components that require `"use client"` and those that must remain server-side.
- Rendering strategy: static, dynamic, SSR, ISR, streaming, client-side rendering.
- Cache: `fetch` cache, `revalidate`, tags, SWR/TanStack Query, invalidation.
- Hydration: possible mismatch sources, browser dependency, dates, random, locale.
- Bundle: client-side imports, heavy libraries, splitting, dynamic imports.

Do not automatically migrate Pages Router to App Router. Do not add
`"use client"` to an entire tree for convenience.

## Open Questions

List only:

- Product decisions needed to avoid an incorrect implementation.
- Technical choices that strongly change architecture, cost, or risk.
- Information without which the plan would be misleading.

For the rest, state assumptions:

- Assumed router if several architectures coexist.
- Package manager selected according to the lockfile.
- Assumed rendering and cache strategy.
- Expected browser compatibility or accessibility level.
- Realistic test coverage.

## Step Breakdown

Order steps to reduce risk:

1. Read configs and detect stack, router, scripts, and conventions.
2. Identify affected components, routes, hooks, services, styles, and tests.
3. Define data, state, rendering, and cache changes.
4. Define UI, accessibility, and design system changes.
5. Define Server Components / Client Components impacts.
6. Plan useful unit, component, and E2E tests.
7. Plan typecheck, lint, test, and build validations.
8. List risks, mitigations, and decisions to confirm.

## Risks To Analyze

- **Rendering**: SSR, SSG, ISR, streaming, unanticipated dynamic rendering.
- **Hydration**: server/client mismatch, browser usage on the server side.
- **Cache**: stale data, missing invalidation, overly broad revalidation.
- **Bundle**: heavy client-side dependency, wrong import, loss of code splitting.
- **Accessibility**: labels, focus, keyboard, contrast, alternative text.
- **TypeScript**: weak contracts, `any`, broken public types.
- **Routing**: dynamic routes, query params, redirects, middleware, API routes.
- **State**: global stores, providers, URL/cache/UI synchronization.
- **Tests**: critical behavior uncovered, fragile mocks, overly broad E2E.
- **Upgrade**: React/Next.js/tooling breaking changes, incompatible plugins.

## Test Strategy

Plan according to the change:

- Unit: pure functions, services, isolatable hooks, validators.
- Components: rendering, interactions, loading/error/empty states, accessibility.
- Next.js: routes, loaders, server actions, route handlers, metadata when relevant.
- E2E: critical journey, navigation, auth, forms, bugfix regression.
- Regression: test that reproduces the bug before the fix when possible.

Do not delete, disable, or loosen a test to hide a regression.

## Technical Validation

Propose only commands adapted to the project and lockfile:

- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test`
- `<package-manager> run test -- <filter>`
- `<package-manager> run test:e2e`
- `<package-manager> run build`
- Storybook or accessibility command if present

Do not propose an unrequested migration, dependency upgrade, or destructive
command.

## Rules

- Never modify code when the user asks only for a plan.
- Do not add a dependency without explicit justification and validation.
- Do not include a broad refactor unless explicitly requested.
- Do not change Next.js router without an explicit request.
- Do not bypass TypeScript, ESLint, tests, or accessibility.
- Respect the package manager imposed by the lockfile.
- Report out-of-scope refactor opportunities without integrating them into the plan.
- Do not hide an important uncertainty: list it as a decision to validate.

## Output Format

Present the plan with:

1. **Summary**: need restated in one sentence.
2. **Assumptions**: assumed choices if not confirmed.
3. **Detected context**: stack, router, conventions, files, and observed constraints.
4. **Steps**: order, file or area, planned change, reason, dependencies.
5. **Files**: files to create or modify, responsibility, and risk.
6. **Tests**: tests to write or adapt, required mocks/fixtures.
7. **Validations**: commands to run and goal of each command.
8. **Risks**: possible impacts and mitigations.
9. **Complexity**: simple, moderate, or complex, with short justification.
10. **Decisions to validate**: only points required before implementation.
