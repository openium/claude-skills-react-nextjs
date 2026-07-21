---
name: test
description: "Adds or fixes unit and component tests for React, TypeScript, and Next.js. Detects Jest or Vitest and React Testing Library, favors behavior-oriented tests, accessible queries, userEvent, async components, hooks, limited mocks, and regression tests."
---

# React / Next.js Tests

## Scope

Create or fix unit and component tests. For E2E, propose coverage only if the
project already uses Playwright, Cypress, or equivalent.

Do not add a new test stack without explicit confirmation.

## Current State

Inspect:

- `package.json`: test scripts and dependencies.
- Jest or Vitest configuration.
- React Testing Library, `@testing-library/user-event`, `jest-dom`.
- `tests/`, `__tests__/`, `*.test.*`, `*.spec.*` structure.
- Global setup, existing mocks, MSW if present.
- Project ability to test Server Components.

## Principles

- Test observable behavior, not implementation.
- Use accessible queries: `getByRole`, `getByLabelText`, `getByText` when relevant.
- Use `userEvent` for interactions.
- Keep tests readable with a clear intent.
- Limit mocks to external boundaries: network, storage, router, clock, external dependency.
- Add a regression test for a bugfix.

## Cases To Cover

- Nominal rendering.
- Loading, empty, error, and disabled states.
- User interactions.
- Async components and promise resolution.
- Hooks through a test component or existing utility.
- Form validation and error messages.
- Server Components only if the project stack supports it.

## Stability

- Avoid assertions on CSS details or fragile structure.
- Control timers, dates, timezone, and random when needed.
- Clean up mocks and side effects.
- Wait for the UI with `findBy*` or `waitFor` when it is truly async.
- Do not increase timeouts to hide a race condition.

## Next.js

- Respect existing mocks for `next/navigation`, `next/router`, `next/image`, `next/link`.
- Distinguish App Router and Pages Router.
- Do not test a Server Action as a simple function if the project has a dedicated helper.

## Do Not

- Do not install Jest, Vitest, RTL, MSW, or Playwright without confirmation.
- Do not mock the entire component under test.
- Do not only test that the component "renders without crashing".
- Do not delete or loosen a test to make the suite pass.
- Do not use `data-testid` if an accessible query fits.

## Output Format

After modification, summarize:

- **Tests added or modified**
- **Behaviors covered**
- **Mocks used**
- **Commands run**
- **Limits or tests still missing**
