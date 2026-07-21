---
name: review
description: "Code review for React, TypeScript, and Next.js across a git diff, branch, PR, files, or given scope. Use to review a change before commit/merge, audit a PR, or check components/routes/hooks/services. Prioritizes bugs, regressions, typing, Server/Client Component boundaries, hooks, security, cache, data fetching, hydration, accessibility, performance, bundle, and missing tests."
---

# React / Next.js Code Review

## Scope

If the user specifies a file, folder, commit, branch, PR, or scope, analyze only
that scope.

Otherwise:

- Analyze the staged diff (`git diff --cached`) if it exists.
- Otherwise, analyze the unstaged diff (`git diff`).
- If no diff exists, ask for the scope to review.

Never modify code during a review unless explicitly requested.

## Process

1. Read `git status` and a diff summary (`git diff --stat` or equivalent).
2. Read the detailed diff for the affected files.
3. Inspect neighboring files only if necessary to prove or rule out a risk.
4. Read useful configs if the risk depends on the project: `package.json`, lockfile, `tsconfig.json`, `next.config.*`, lint, and tests.
5. Detect App Router, Pages Router, or hybrid architecture if a route, rendering, or cache is involved.
6. Report only issues proven by code, configuration, or diff.

Do not report a speculative risk without a credible execution path.

## Severities

- **Blocking**: proven bug, security flaw, data leak, regression, broken hydration, runtime crash, hidden typing error, public contract break.
- **Important**: real and plausible risk, missing test for critical behavior, blocking accessibility issue, likely performance or cache degradation.
- **Suggestion**: readability, convention, simplification, additional test, or improvement without immediate risk.

## Analysis Points

### Bugs And Regressions

- Behavior change that is not covered or not announced.
- Broken edge case: `null`, `undefined`, empty array, empty string, extreme value.
- Loading/error/empty state removed or inconsistent.
- Swallowed error, misleading fallback, or unhandled promise.
- Props, API payload, route, query param, or response contract changed without compatibility.

### TypeScript Typing

- `any`, `as` assertion, or non-null assertion `!` that hides a real case.
- Public type changed without adapting consumers.
- Server/client types confused: serializable data, `Date`, `Map`, functions, classes.
- Generics or unions that lose error cases.
- Disabled TypeScript rules or typechecker bypass.

### Server / Client Component Boundaries

- `"use client"` added too high in the tree.
- Client hook, browser API, or event handler used in a Server Component.
- Secret, token, sensitive cookie, or server logic exposed in a Client Component.
- Non-serializable props passed from a Server Component to a Client Component.
- Server component rendering made dependent on browser state.

### Hooks And Effects

- Missing or unnecessary dependencies in `useEffect`, `useMemo`, `useCallback`.
- Effect used to derive state that can be computed during render.
- Race condition, uncancelled concurrent request, or state update after unmount.
- Hook called conditionally or in a loop.
- State duplicated between URL, store, cache, and component without a clear source of truth.

### Security And Data Exposure

- Secret, token, API key, DSN, or personal data exposed client-side.
- Variable turned into `NEXT_PUBLIC_*` without proof that it may be public.
- Reading, displaying, or committing `.env` or sensitive environment file.
- HTML injected through `dangerouslySetInnerHTML` without proven sanitation.
- Route handler, API route, server action, or middleware without expected access control.
- Redirect, external URL, or user parameter not validated.
- Log or analytics containing sensitive data.

### Cache And Data Fetching

- `fetch` cache/revalidate strategy inconsistent with expected freshness.
- Missing invalidation after mutation: tags, paths, SWR/TanStack Query.
- User or tenant data cached too broadly.
- Request duplicated between server and client.
- Avoidable waterfall or fetch in a client component when the server fits.
- Error, loading, or retry handling incompatible with the existing experience.

### Hydration And Rendering

- Likely mismatch: date, locale, random, timezone, `window`, screen size.
- Markup difference between server and client.
- Client component depending on an unstable value on first render.
- SSR/SSG/ISR/dynamic usage incompatible with the data.
- Suspense, streaming, or fallback hiding a user-facing error.

### Accessibility

- Field without accessible label or useful description.
- Button/link without accessible name or wrong interactive role.
- Broken keyboard navigation, visible focus, or tab order.
- Modal, menu, or popover without appropriate focus/escape/aria handling.
- Informative image without alternative text.
- Error message not associated with the field or not announced.

### Performance And Bundle

- Heavy import in a Client Component or critical path.
- Lost code splitting, missing dynamic import, or inflated shared bundle.
- Avoidable global re-render through provider, store, or unstable props.
- Expensive calculation on every render without need.
- Images not optimized, missing dimensions, or incorrect priority usage.
- API/client N+1, missing pagination, or overly large payload.

### Tests

- Critical behavior added without a test.
- Bugfix without a regression test.
- Interactive component without interaction or error-state test.
- Critical route or data fetching without a suitable integration/E2E test.
- Mock that does not reflect the real contract.
- Test deleted, disabled, or loosened to make validation pass.

## Do Not Report

- Missing test if the diff does not change behavior.
- Convention if the project clearly follows a different convention.
- Stylistic improvement without impact when more important issues exist.
- The same issue repeated line by line: group the remark.
- App Router/Pages Router preference unrelated to a real bug.
- Unrequested migration, dependency, or refactor.

## Output Format

Present findings first, sorted by descending severity.

For each finding:

- **File and line**
- **Severity**: blocking, important, or suggestion
- **Evidence**: excerpt or fact verified in the code, configuration, or diff
- **Impact**: concrete consequence for the user, security, stability, or maintenance
- **Fix**: recommended change without applying code
- **Recommended test**: test or validation that would cover the issue

After findings, add if useful:

- **Open questions**: only the information required to conclude.
- **Review limits**: tests not run, incomplete scope, dependency on a remote PR.

If no issue is found, say so clearly and mention any tests not run or review
limits.
