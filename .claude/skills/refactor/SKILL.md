---
name: refactor
description: "Safely refactors React, TypeScript, and Next.js code. Use to simplify components, hooks, types, duplication, derived state, unnecessary effects, or Server/Client Component boundaries, with minimal diff and characterization tests."
---

# React / Next.js Refactoring

## Scope

If the user specifies a file, component, hook, folder, or diff, limit the
refactor to that scope.

If the scope is ambiguous or too broad, ask for a target before modifying.

## Goal

Improve structure without changing observable behavior:

- same public props;
- same useful rendering;
- same interactions;
- same side effects;
- same routes, payloads, errors, and user states.

## Process

1. Read the target code and existing tests.
2. Search for incoming usages of the component, hook, or service.
3. Identify local conventions before applying a general rule.
4. Add or propose a characterization test if behavior is not covered.
5. Apply the smallest coherent refactor.
6. Run targeted tests when possible.

## Covered Refactors

- Component too complex: extract private subcomponents or pure functions.
- Hook too loaded: separate state, effects, data fetching, and handlers.
- Duplication: extract only if at least two real usages exist.
- Types: clarify props, unions, derived types, without weakening typing.
- Derived state: replace unnecessary state with render-time calculation or justified memo.
- Unnecessary effects: remove avoidable synchronization effects.
- Server/Client Components: reduce client surface without changing UX.
- Tests: preserve behavior through existing tests or characterization.

## Server / Client Boundaries

- Do not add `"use client"` to an entire tree to simplify.
- Do not move server logic into a Client Component.
- Do not expose a secret, cookie, token, or internal client to the browser.
- Preserve serializability of props passed to Client Components.

## Tests And Validation

- Run nearby unit or component tests.
- Run typecheck if signatures, props, or types change.
- Run lint if imports, hooks, or conventions are touched.
- Add a characterization test when the refactor touches uncovered behavior.

## Do Not

- Do not migrate framework, router, or architecture.
- Do not change a dependency or add a library.
- Do not broadly rewrite a module by preference.
- Do not mix refactor and feature.
- Do not modify behavior, payload, accessibility, or route without an explicit request.
- Do not make opportunistic improvements outside the scope.

## Output Format

Summarize:

- **Modified files**
- **Applied refactors** and short reason
- **Preserved behavior**
- **Tests or validations run**
- **Remaining risks or limits**
