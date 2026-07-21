---
name: observability
description: "Audits or improves observability in a React/Next.js project. Covers server and browser errors, structured logs, request correlation, traces, metrics, source maps, Sentry or an existing tool, alerts, sampling, token/cookie/personal data protection, and expected/unexpected error distinction."
---

# React / Next.js Observability

## Scope

Determine whether the user is asking for:

- An audit of logs, errors, traces, or metrics.
- Adding logs around a flow, route, Server Action, API route, or external client.
- Diagnosis of missing production information.
- Integration or correction of Sentry, OpenTelemetry, Datadog, or an existing tool.
- Noise reduction or a sensitive data leak.

Do not read or display `.env`, DSNs, tokens, cookies, or personal data.

## Current State

Inspect according to the project:

- `package.json`: Sentry, OpenTelemetry, Datadog, logger, analytics.
- `next.config.*`, instrumentation, middleware, source maps.
- Route Handlers, API routes, Server Actions, external clients.
- Error boundaries, `error.tsx`, `global-error.tsx`, browser logs.
- CI/CD and deployment configuration if visible.

## Goals

Good observability should make it possible to know:

- What failed?
- On the server, browser, edge, or external dependency side?
- For which request, route, tenant, resource, or action, without sensitive data?
- Expected, business, transient, or incident error?
- How to correlate logs, traces, metrics, and frontend events?

## Logs And Errors

- Use structured logs with stable context.
- Do not log tokens, cookies, Authorization, a complete sensitive payload, or unnecessary PII.
- Distinguish expected and unexpected errors.
- Do not swallow a critical exception after logging it.
- On the client side, avoid capturing sensitive form data.

## Traces, Metrics, Alerts

- Propagate request id or correlation id if the project does it.
- Instrument boundaries: route, server action, external client, async job.
- Measure error rate, latency, timeouts, critical failures.
- Keep cardinality controlled: no email, massive user UUID, or dynamic payload as a label.
- Plan actionable alerts and appropriate sampling.

## Source Maps And Tools

- Use Sentry or the existing tool instead of changing tools.
- Check source map upload without unnecessarily exposing public sources.
- Keep environments, release, and commit SHA consistent if already used.

## Tests And Validation

- Test that an expected error is not reported as an incident when possible.
- Test that a critical error is surfaced or logged with useful context.
- Verify that sensitive data is not included in logs/events.
- Do not trigger production alerts or real external calls without confirmation.

## Do Not

- Do not add an observability dependency without an explicit request.
- Do not log secrets to debug.
- Do not replace an exception with a simple log.
- Do not send personal data to a third-party tool without filtering.
- Do not create metrics or tags with uncontrolled cardinality.

## Output Format

Present:

- **Findings** by severity, with file/line, evidence, impact, fix.
- **Context to add or remove**
- **Sensitive data protected**
- **Recommended tests or checks**
- **Remaining limits**
