---
name: security
description: "Audits the security of a React and Next.js project. Covers XSS, server validation, authentication, authorization, IDOR, CSRF according to architecture, open redirects, injection, uploads, cookies, headers, Server Actions, API routes, NEXT_PUBLIC_*, secrets in the bundle, vulnerable dependencies, and sensitive logs."
---

# React / Next.js Security

## Scope

By default, analyze modified files. If the user specifies a file, folder, route,
PR, or flow, limit the audit to that scope.

Never read, display, or copy `.env` or sensitive environment files. Never
display a secret during the audit.

## Context To Inspect

- `package.json` and lockfile for dependencies.
- `next.config.*`, middleware, headers, redirects.
- Route Handlers, API routes, Server Actions, auth services.
- Client components exposing data.
- Logs, analytics, observability.
- Uploads, forms, runtime validations.

## Analysis Criteria

### XSS

- `dangerouslySetInnerHTML` without proven sanitization.
- Injection into URL, HTML, Markdown, script, analytics, or attribute.
- User data rendered in an unescaped context.

### Server Validation

- Client-side-only validation.
- External payload processed without runtime validation.
- TypeScript used as the only barrier for runtime input.

### Authentication, Authorization, IDOR

- Sensitive API route, Server Action, or page without access control.
- Resource access by id without checking owner, tenant, or role.
- Excessive user data returned to the client.

### CSRF, Cookies, And Headers

- Cookie/session mutation without protection suited to the architecture.
- Sensitive cookie without appropriate `HttpOnly`, `Secure`, `SameSite`.
- Security headers absent or weakened if the project manages them.

### Redirects And Injection

- Open redirect through an unvalidated parameter.
- External URL, command, regex, or query built with unvalidated user input.

### Uploads

- MIME type, extension, size, or filename not validated server-side.
- Upload into a dangerous public path.
- File processed without antivirus/sandbox if the context requires it.

### Next.js And Bundle

- Secret imported into a Client Component.
- Variable passed as `NEXT_PUBLIC_*` without proof that it may be public.
- Token, cookie, DSN, or API key in the bundle.
- Server Action exposing an operation without validation or authorization.

### Dependencies And Logs

- Vulnerable dependency or missing lockfile.
- Personal data, tokens, cookies, or Authorization in logs/analytics.
- Public source maps exposing sensitive information if relevant to the project.

## Do Not

- Do not display a secret to prove a leak.
- Do not read `.env`.
- Do not fix automatically without a request.
- Do not recommend a security dependency without justification and validation.
- Do not ignore server validation because client validation exists.

## Output Format

For each vulnerability:

- **File and line**
- **Category**
- **Severity**: blocking, important, or suggestion
- **Verified evidence** without copying a secret
- **Realistic exploitation scenario**
- **Proposed fix**
- **Regression test or check**
