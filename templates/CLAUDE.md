# CLAUDE.md

Project instructions for Claude Code.

This file must remain short, concrete, and maintained with the project. It
complements the specialized skills available in `.claude/skills/`.

## Project Context

- Project name: `<project-name>`
- Business domain: `<short description>`
- Application type: `<marketing site, SaaS, back office, e-commerce, mobile web app, library>`
- Main environment: `<local Node.js, Docker, CI only, cloud platform>`
- Criticality level: `<low, medium, high>`

## Technical Stack

- Node.js: `<version detected in .nvmrc, package.json, Dockerfile, or CI>`
- Package manager: `<npm, pnpm, or Yarn according to the lockfile present>`
- React: `<version>`
- Next.js: `<version>`
- Router: `<App Router, Pages Router, or hybrid architecture>`
- TypeScript: `<strict, non-strict, specific configuration>`
- Styles and design system: `<CSS Modules, Tailwind CSS, Sass, styled-components, MUI, shadcn/ui, other>`
- State management: `<React state, Context, Redux, Zustand, Jotai, TanStack Query, other>`
- Data fetching: `<Server Components, fetch, API routes, Route Handlers, SWR, TanStack Query, GraphQL, other>`
- Unit, component, and E2E tests: `<Vitest, Jest, Testing Library, Storybook, Playwright, Cypress, other>`
- Observability: `<Sentry, Datadog, OpenTelemetry, platform logs, analytics, none>`

## General Modification Rules

- Respond in concise, actionable technical English.
- Read the code, scripts, and configuration files before modifying.
- Preserve the conventions and versions actually detected in the project.
- Use the package manager that matches the lockfile.
- Do not assume App Router, Pages Router, React Server Components, or a recent version without proof.
- Do not automatically migrate from Pages Router to App Router.
- Do not add `"use client"` to an entire tree for convenience.
- Do not disable TypeScript, ESLint, or tests to make validation pass.
- Do not perform opportunistic refactors unrelated to the request.

## Project Startup Generation Conventions

These rules apply when new code is generated from the boilerplate (startup
phase, before developers take over). They are prescriptive: apply them by
default, without waiting for existing code to impose them. Goal: deliver code
that developers can integrate without renaming or retranslating everything.

### Code Language Versus Interface Language

- All **code** is in English, without exception: file names, folders, routes,
  functions, components, hooks, variables, types, and **object properties**.
- **French** exists only in translation files. Never in an identifier, route
  name, or object key.
- A French business label in the request does not become a French code name:
  translate it to English for code naming, keep French for displayed text.

### Data Schema: Ask This Question First

Before generating types, routes, or data services, ask:

> "Is there a database schema or backend API contract I should align with?"

- If yes: frontend types and properties use **exactly** the backend names
  (including names). Never map or transform the API response to "translate" or
  "rearrange" fields, because that forces later rewrites. A truly necessary UI
  to API conversion (for example, boolean toggle to enum) belongs in the service
  layer, not in the component.
- If no: name according to common English REST/API conventions and note that
  these names must be confirmed when the real backend arrives.

### Translations From Generation

- No hardcoded visible text. All displayed text goes through `next-intl` as soon
  as the component is written.
  - Server Components: `const t = await getTranslations('namespace')`
  - Client Components: `const t = useTranslations('namespace')`
- Each key is added to `messages/fr.json` **and** `messages/en.json` at the same
  time, under a namespace matching the section.
- In JSX: single quotes and braces: `{t('key')}`, not `t("key")` or
  `{t("key")}`. To inject JSX into a string, use `t.rich('key', { ... })`.

### `generateMetadata` On Every Page

Every page exports `generateMetadata`, powered by a translation key
(`<namespace>.titleMetadata`): no page without a browser tab title.

```ts
import type { Metadata } from 'next';
import { getTranslations } from 'next-intl/server';

export async function generateMetadata(props: {
  params: Promise<{ lng: string }>;
}): Promise<Metadata> {
  const { lng } = await props.params;
  const t = await getTranslations({ locale: lng, namespace: 'myNamespace' });
  return { title: t('titleMetadata') };
}
```

The `titleMetadata` key is added to `messages/fr.json` and `messages/en.json`.

### Zero Duplication

As soon as JSX or logic blocks are identical or nearly identical, extract a
reusable component or helper instead of copy-pasting.

### Section-Level Loading, Not Global Loader

- Do not block an entire page on a single `Promise.all` with a full-screen
  loader.
- Split independent data calls into subcomponents, each with its own `Suspense`
  and skeleton, to display each block as soon as its data arrives (streaming)
  instead of waiting for the slowest data.

### Component Gallery

For every addition or modification of a reusable component, reference it in a
gallery page that shows all its states (loading, empty, error, disabled,
variants). This page serves as visual verification for the designer and
developers.

## Security And Sensitive Data

- Never read, display, or commit `.env` or other sensitive environment files.
- Never expose secrets, tokens, passwords, cookies, API keys, DSNs, or sensitive personal data.
- Do not turn a variable into `NEXT_PUBLIC_*` without verifying that it may be public in the browser.
- Do not add realistic credentials to code, tests, fixtures, or documentation.

## Workflow Before, During, And After Modification

Before modifying:

1. Identify the exact need.
2. Read the relevant files and local conventions.
3. Check the detected stack, the Next.js router in use, and the lockfile.
4. Propose a short plan if the change touches several areas.

During modification:

- Keep the diff minimal.
- Respect Next.js server/client boundaries.
- Add or adapt tests when behavior changes.
- Preserve existing names, patterns, and abstractions.

After modification:

- Summarize the modified files.
- Indicate the commands run.
- Report tests that were not run or could not be run.
- Mention residual risks if necessary.

## Project Commands

Adapt this section to the project and the detected package manager.

```bash
# Install dependencies
<npm install | pnpm install | yarn install>

# Start the development server
<npm run dev | pnpm dev | yarn dev>

# Check types
<npm run typecheck | pnpm typecheck | yarn typecheck>

# Lint
<npm run lint | pnpm lint | yarn lint>

# Unit and component tests
<npm run test | pnpm test | yarn test>

# E2E tests
<npm run test:e2e | pnpm test:e2e | yarn test:e2e>

# Production build
<npm run build | pnpm build | yarn build>
```

## Available Skills

Adapt this list to the skills actually installed in `.claude/skills/`.

- `<skill-review>`: `<usage>`
- `<skill-debug>`: `<usage>`
- `<skill-test>`: `<usage>`
- `<skill-nextjs>`: `<usage>`
- `<skill-performance>`: `<usage>`
- `<skill-accessibility>`: `<usage>`

## React, TypeScript, And Next.js Conventions

- Components: `<organization, naming conventions, co-location>`
- Server Components: prefer server rendering when compatible with the need.
- Client Components: limit `"use client"` to components that need browser state, effects, or client APIs.
- Routing: respect the existing App Router, Pages Router, or hybrid architecture.
- Data fetching: follow the patterns already used for cache, revalidation, errors, and loading states.
- TypeScript: avoid `any`; use existing types and make public contracts explicit.
- Forms: `<React Hook Form, server actions, Zod validation, other>`
- Accessibility: preserve labels, focus states, keyboard navigation, and alternative text.
- Performance: avoid unnecessary client bundles, heavy client-side imports, and unnecessary recomputations.

## Tests

- Unit: `<location and command>`
- Components: `<location and command>`
- E2E: `<location and command>`
- Fixtures and mocks: `<conventions>`

When a bug is fixed, add a regression test if the project allows it. Do not
delete or loosen a test to hide a regression.

## Git, Branches, Commits, Pull Requests, And Deployments

- Do not modify files unrelated to the request.
- Do not revert user changes without an explicit request.
- Do not include IDE files, caches, logs, generated artifacts, or environment files.
- Do not run destructive Git or data commands without explicit confirmation.
- Check staged and unstaged diffs before committing.
- Document the verification commands run and known risks in the pull request.
- Do not create or push a tag, branch, or deployment without an explicit request.

### Branches

- Create working branches from the project's integration branch.
- Use the associated ticket number in the branch name.
- Use a short kebab-case name after the ticket.

Recommended format:

```text
<type>/<ticket>_<short-description>
```

Examples:

```text
feature/US-123_login-form
fix/TICKET-456_query-params
hotfix/INC-789_checkout-error
chore/TECH-321_update-next-config
```

### Commits

- Use the Conventional Commits convention.
- Use the associated ticket number as the scope.
- If no ticket exists, ask for the expected scope or use a short technical scope validated by the project.

Recommended commit format:

```text
<type>(<scope>): <short description>
```

Examples:

```text
feat(US-123): add login form
fix(TICKET-456): preserve query params
test(TICKET-456): add router regression test
docs(TECH-321): document local setup
```

## Project-Specific Notes

Add important local decisions here:

- `<decision 1>`
- `<decision 2>`
- `<decision 3>`
