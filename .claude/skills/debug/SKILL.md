---
name: debug
description: "Diagnoses a frontend, React, TypeScript, or Next.js error without automatically applying a fix. Use for broken builds, server or browser errors, hydration mismatches, render loops, routing, stale cache, Server/Client Components, flaky tests, or dev/prod gaps."
---

# React / Next.js Debugging

## Scope

If the user provides an error, stacktrace, log, failing test, URL, screenshot, or
file, start with that signal.

Otherwise:

- Read `git status` to identify recent changes.
- Identify scripts in `package.json`.
- Inspect the stack: lockfile, `tsconfig.json`, `next.config.*`, tests.
- Ask for the exact symptom if no usable signal is present.

Do not automatically apply a fix. Propose the fix after establishing the root
cause, unless the user explicitly asks you to modify the code.

## Reproducible Method

Always structure the investigation:

1. **Symptom**: message, command, page, environment, browser, test.
2. **Reproduction**: smallest command or action sequence that triggers the bug.
3. **Hypotheses**: possible causes, sorted by probability.
4. **Evidence**: files, lines, logs, diff, config, or observed behavior.
5. **Root cause**: minimal cause that explains the symptom.
6. **Proposed fix**: targeted change, without opportunistic refactor.
7. **Regression test**: test or validation that proves the fix.

If reproduction is impossible, state what is missing and what data to collect.

## Analysis Points

### Build And TypeScript

- `next build`, `tsc`, ESLint, or bundler error.
- Modified public type, `any`, `as` assertion, non-null assertion, or bypassed TS config.
- Difference between `next dev` and `next build`.
- Server/client incompatible import or module missing in the target environment.

### Server And Browser Errors

- Read the first application frame.
- Distinguish Node.js, Route Handler, Server Action, middleware, and browser errors.
- Check that secrets are not displayed in logs or the response.
- Compare local, CI, preview, and production environments if the bug depends on context.

### Hydration Mismatch

- Look for date, random, locale, timezone, `window`, media query, storage, screen size.
- Check server/client markup and values that are unstable on first render.
- Identify whether the component should remain server-side or become local client-side code.

### Hooks And Render Loops

- `useEffect`, `useMemo`, `useCallback` dependencies.
- State update during render or effect that rewrites its own dependency.
- Conditional hook, race condition, uncancelled request.
- Derived state stored unnecessarily.

### Routing

- App Router: layouts, pages, route handlers, dynamic segments, `notFound`, `redirect`.
- Pages Router: `getServerSideProps`, `getStaticProps`, API routes, query params.
- Hybrid architecture: check the area actually concerned.
- Middleware, redirects, trailing slash, basePath, i18n if configured.

### Cache And Stale Data

- `fetch` cache, `revalidate`, tags, `revalidatePath`, `revalidateTag`.
- SWR/TanStack Query if already used.
- Browser cache, CDN, edge, ISR, per-user or per-tenant data.
- Invalidation after mutation.

### Server / Client Components

- Browser API or client hooks in a Server Component.
- Secret or server logic imported client-side.
- Non-serializable props between server and client.
- `"use client"` placed too high to hide the problem.

### Flaky Tests

- Unawaited async behavior, timers, races, test order, global mock.
- Testing Library query that is not accessible or assertion too tied to implementation.
- Gap between test environment and Next.js runtime.

## Useful Commands

Adapt to the existing lockfile and scripts:

- `<package-manager> run build`
- `<package-manager> run typecheck`
- `<package-manager> run lint`
- `<package-manager> run test -- <filter>`
- `<package-manager> run test:e2e -- <filter>`
- `next build` only if the project uses it directly

Do not run destructive commands, migrations, data resets, or global cleanup
without explicit confirmation.

## Do Not

- Do not hide an error behind a silent fallback.
- Do not delete or loosen a test without proof that it is obsolete.
- Do not disable TypeScript, ESLint, hydration warnings, or build checks.
- Do not clear cache as a durable solution without a proven cause.
- Do not expose secrets, cookies, tokens, or personal data.
- Do not turn a variable into `NEXT_PUBLIC_*` without proof that it is public.

## Output Format

Respond with:

1. **Symptom**: error or observed behavior.
2. **Reproduction**: minimal command or scenario.
3. **Hypotheses**: sorted by probability if the cause is not proven.
4. **Evidence**: files, lines, logs, or command results.
5. **Root cause**: one-sentence diagnosis with confidence level.
6. **Proposed fix**: targeted change to apply.
7. **Regression test**: test or validation to add/run.
