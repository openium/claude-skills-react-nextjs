---
name: performance
description: "Audits and optimizes React/Next.js performance with measurements before optimization. Covers React rendering, hydration, Server/Client Components, JavaScript bundle, imports, images, fonts, cache, request waterfalls, Core Web Vitals, large lists, justified memoization, and production build."
---

# React / Next.js Performance

## Scope

If the user specifies a page, component, route, diff, or metric, analyze only
that scope. Otherwise ask for the symptom: slowness, bundle, LCP, CLS, INP,
hydration, requests, build.

Do not modify code during an audit unless explicitly requested.

## Measure Before Optimizing

Look for evidence:

- Core Web Vitals: LCP, CLS, INP, TTFB.
- JS size by route or chunk.
- Number of renders and React cost.
- Hydration duration.
- Network requests and waterfalls.
- Cache hit/miss or revalidation.
- Image/font size and dimensions.
- `next build` time.

If direct measurement is impossible, clearly separate hypothesis and evidence.

## Analysis Points

- React rendering: global re-renders, unstable props, providers too high.
- Hydration: excessive client surface, mismatch, heavy components.
- Server/Client Components: unnecessary client logic, server/client imports.
- Bundle: heavy dependencies, barrel imports, dynamic import, tree shaking.
- Images and fonts: `next/image`, dimensions, formats, priority, preload, font display.
- Cache: `fetch` cache, ISR, CDN, SWR/TanStack Query, invalidation.
- Request waterfalls: waterfalls, server/client duplication, lack of parallelization.
- Large lists: pagination, virtualization if already accepted, bounded rendering.
- Memoization: `memo`, `useMemo`, `useCallback` only with proven cause.
- Production build: analyze `next build`, analyzer if already configured.

## Expected Fixes

- Reduce the client surface before memoizing everywhere.
- Move to the server what does not need the browser.
- Split heavy imports or load on demand.
- Add cache with a clear invalidation strategy.
- Optimize images/fonts according to existing primitives.
- Remove proven duplicate requests or waterfalls.

## Do Not

- Do not optimize without evidence or an explicitly stated hypothesis.
- Do not add speculative memoization.
- Do not add cache without defined invalidation or freshness.
- Do not degrade accessibility or business behavior to save time.
- Do not add an analysis or runtime dependency without confirmation.

## Output Format

Present findings by severity:

- **File and line**
- **Type**: rendering, hydration, bundle, image, cache, network, Core Web Vitals
- **Evidence or measurement**
- **Impact**
- **Root cause**
- **Proposed fix**
- **Recommended validation**
- **Expected gain** if estimable
