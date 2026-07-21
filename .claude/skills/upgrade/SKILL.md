---
name: upgrade
description: "Plans or supports upgrades for Node.js, React, Next.js, TypeScript, and frontend dependencies. Analyzes versions, official release notes, breaking changes, peer dependencies, codemods, lockfile, tests, build, rollback, App Router, and Pages Router. Does not run the upgrade without explicit plan validation."
---

# React / Next.js Upgrade

## Scope

Determine whether the user is asking for:

- Upgrade diagnosis or plan: do not modify code.
- Application of a validated upgrade: proceed in verifiable steps.
- Specific target: Node.js, React, Next.js, TypeScript, dependency, or tooling.

If the target is not given, analyze the current state and ask for the target
before modifying.

Do not run the upgrade without explicit plan validation.

## Current State

Inspect:

- `package.json`, lockfile, package manager.
- Node.js versions: `.nvmrc`, `.node-version`, Dockerfile, CI, `engines`.
- React, Next.js, TypeScript, ESLint, test runner, bundler.
- App Router, Pages Router, or hybrid.
- `next.config.*`, `tsconfig.json`, lint, tests, CI.
- Dependencies with sensitive peer dependencies.

## Sources

- Read official release notes and guides for the target.
- Check breaking changes and documented migrations.
- Check official codemods if available.
- Use primary sources when information may have changed.

## Strategy

- Prefer incremental upgrades.
- Separate Node.js, Next.js, React, TypeScript, and risky dependencies when possible.
- Respect the existing lockfile and package manager.
- Do not modify the lockfile by hand.
- Plan a rollback strategy.
- Test App Router and Pages Router according to the real architecture.

## Watch Points

- Next.js breaking changes: config, routing, cache, image, middleware, server actions.
- React: rendering, strict mode, hooks, hydration, types.
- TypeScript: stricter options, React/Node types.
- Peer dependencies: eslint, testing-library, Storybook, bundler.
- Codemods: run them in dry-run mode when possible, review the diff.
- CI/CD: Node version, dependency cache, build command.

## Useful Commands

Adapt to the project:

- `<package-manager> outdated`
- `<package-manager> why <package>`
- `<package-manager> install --lockfile-only` if validated
- Official codemod in dry-run mode if available
- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test`
- `<package-manager> run build`

Do not run an effective update command without explicit validation.

## Do Not

- Do not make an unrequested major jump.
- Do not mix upgrade and broad refactor.
- Do not change Next.js router.
- Do not add or remove a dependency without proof and validation.
- Do not ignore peer dependencies or broken tests.
- Do not modify `.env` or secrets.

## Output Format

For a diagnosis:

- **Current state**
- **Requested or recommended target**
- **Breaking changes**
- **Peer dependencies and blockers**
- **Impacted files**
- **Ordered execution plan**
- **Tests/build**
- **Rollback**
- **Decisions to validate**

After validated application:

- **Modified files**
- **Updated dependencies**
- **Applied fixes**
- **Commands run**
- **Risks or remaining steps**
