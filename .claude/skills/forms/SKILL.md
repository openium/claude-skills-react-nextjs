---
name: forms
description: "Creates or modifies React/TypeScript/Next.js forms according to the existing architecture. Covers controlled/uncontrolled patterns, client and server validation, runtime validation, Server Actions or APIs, errors, focus, double submission, loading/disabled states, accessibility, sensitive data, and tests."
---

# React / Next.js Forms

## Scope

Create, fix, or improve a form without assuming React Hook Form, Zod, Yup, or
another library if it is not already used.

## Current State

Inspect:

- Neighboring forms and validation conventions.
- Presence of React Hook Form, Formik, Zod, Yup, Valibot, or custom validators.
- Submission architecture: Server Actions, Route Handlers, API routes, client API.
- Existing error, loading, disabled, and focus handling.
- Existing form tests.

## Form Model

- Use controlled or uncontrolled inputs according to the project convention.
- Avoid storing two sources of truth for the same value.
- Preserve TypeScript types and payload contracts.
- Do not expose sensitive data client-side more than necessary.

## Validation

- Keep server validation mandatory for user data.
- Add client validation only as UX help.
- Use runtime validation if the project already has it or if the boundary requires it.
- Return field errors and global errors.
- Do not trust TypeScript types to validate runtime input.

## Submission And States

- Prevent double submission if the action is not idempotent.
- Handle loading and disabled states without unnecessarily blocking keyboard navigation.
- Preserve useful values after an error.
- Distinguish validation error, network error, and unexpected server error.
- Handle success, reset, and redirect according to the existing journey.

## Accessibility And Focus

- Associate label, description, and error message with the field.
- Use `aria-invalid` and `aria-describedby` when useful.
- Move focus to the first error or provide an accessible summary.
- Do not replace a native submit button with a non-semantic element.

## Next.js

- Use Server Actions if the project already uses them and the form fits.
- Use an API route or Route Handler if that is the existing convention.
- Do not migrate Pages Router to App Router for a form.
- Do not move a secret or sensitive validation into a Client Component.

## Tests

- Test user input with `userEvent`.
- Test client validation if present, server errors, loading, and double submit.
- Test the sent payload or Server Action through the existing strategy.
- Add a regression test for bugfixes.

## Do Not

- Do not assume React Hook Form or a schema library.
- Do not validate only on the client side.
- Do not expose a token, secret, or sensitive data in the DOM or logs.
- Do not disable a button without a perceptible message or state.
- Do not remove useful native attributes (`type`, `required`, `autoComplete`) without a reason.

## Output Format

Summarize:

- **Form architecture**
- **Client/server validation**
- **Error and focus handling**
- **Loading/disabled states**
- **Tests added or proposed**
- **Risks or decisions to validate**
