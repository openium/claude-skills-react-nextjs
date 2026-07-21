---
name: data-fetching
description: "Designs or fixes data fetching in a React/Next.js project. Covers server or client loading, existing strategy, cache, revalidation, invalidation, errors, cancellation, concurrency, pagination, deduplication, sensitive data, and loading/empty states."
---

# React / Next.js Data Fetching

## Scope

Handle server-side or client-side data loading according to the project's
existing architecture.

Do not introduce TanStack Query, SWR, a GraphQL client, or another dependency if
the project does not already use it, unless explicitly confirmed.

## Current State

Inspect:

- Router in use: App Router, Pages Router, or hybrid.
- Existing patterns: `fetch`, Server Components, Route Handlers, API routes, Server Actions.
- Existing libraries: TanStack Query, SWR, Apollo, urql, custom client.
- Cache strategy: `revalidate`, tags, client cache, CDN, ISR.
- Existing error handling and loading/empty states.
- Data types and presence of sensitive data.

## Server Or Client Choice

- Prefer server loading for initial data, SEO, security, and bundle reduction.
- Prefer client loading for frequent interactions, highly dynamic data, or local user state.
- Do not duplicate the same request on the server and client without a reason.
- Do not expose tokens, secrets, or sensitive data to the browser.

## Cache, Revalidation, And Invalidation

- Define expected freshness, user/tenant scope, and invalidation.
- For App Router, explicitly choose cache, `no-store`, `revalidate`, tags, or path/tag invalidation.
- For SWR/TanStack Query, respect existing query keys, stale time, and invalidation.
- Avoid globally caching personalized data.

## Errors, Concurrency, And Cancellation

- Handle expected and unexpected errors.
- Add retry only if the project already does it and the operation is idempotent.
- Cancel or ignore obsolete responses on the client side.
- Avoid race conditions between filters, pagination, and mutation.

## Pagination And Deduplication

- Add pagination, a limit, or infinite loading if volume can grow.
- Deduplicate identical requests when the stack allows it.
- Avoid server/client waterfalls.
- Keep query params as the source of truth if the project uses them that way.

## UI States

Cover:

- initial loading;
- refresh/revalidation;
- empty;
- error;
- finished pagination;
- disabled during mutation if needed.

## Section-Level Loading

- Do not block an entire page on a single `Promise.all` with a full-screen
  loader.
- Split independent calls into subcomponents, each with its own `Suspense` and
  skeleton, to display each block as soon as its data arrives (streaming) instead
  of waiting for the slowest request.
- Reserve the global loader for cases where all data is genuinely
  interdependent.

## Tests And Validation

- Test success, error, empty, and pagination cases when applicable.
- Mock only the external boundary: `fetch`, API client, MSW if already used.
- Check typecheck, component tests, and build.

## Do Not

- Do not add TanStack Query or SWR if absent without confirmation.
- Do not make a variable or secret public to call an API on the client side.
- Do not cache user data in a shared cache.
- Do not hide errors behind an empty array.
- Do not refactor the data layer outside the scope.

## Output Format

Present:

- **Detected strategy**
- **Server/client choice**
- **Cache and invalidation**
- **States and errors**
- **Files to modify**
- **Tests and validations**
- **Risks or decisions to validate**
