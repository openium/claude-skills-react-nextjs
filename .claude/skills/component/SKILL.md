---
name: component
description: "Creates or modifies a React/TypeScript/Next.js component that complies with the existing design system. Use for UI components, prop APIs, variants, composition, responsive behavior, loading/empty/error/disabled states, accessibility, focus, tests, and Storybook if already installed."
---

# React / Next.js Components

## Scope

Create or modify a component while respecting the existing design system,
patterns, and dependencies.

Do not introduce a new UI library without explicit confirmation.

## Current State

Before writing:

- Search for neighboring components and similar usages.
- Identify the style system: CSS Modules, Tailwind, Sass, styled-components, MUI, shadcn/ui, other.
- Check folder, export, test, and Storybook conventions.
- Detect whether the component should be a Server Component or Client Component.
- Read existing tokens, variants, or UI primitives.

## Component API

- Define minimal, typed, and stable props.
- Prefer composition (`children`, existing slots) over an overly broad API.
- Handle variants only if they match the design system.
- Name props according to existing components.
- Avoid exposing internal style or state details.

## UI States

Cover as needed:

- loading;
- empty;
- error;
- disabled;
- selected/active;
- focus/hover;
- responsive.

Do not invent states if the existing design does not account for them, but call
out gaps that block usage.

## Accessibility And Focus

- Use semantic HTML before ARIA.
- Provide a label, accessible name, and description when needed.
- Handle visible focus, keyboard order, `aria-*`, and `role` only when useful.
- For modals, menus, popovers: initial focus, focus return, Escape, keyboard navigation.
- Do not replace a button with a clickable `div`.

## Next.js

- Keep a Server Component by default if no client API is required.
- Add `"use client"` only to the file that uses state, effects, events, or browser APIs.
- Do not import a server module into a client component.
- Use `next/image`, `next/link`, or existing primitives according to the project.

## Component Gallery

If the project has a gallery page (started from a boilerplate), reference every
reusable component created or modified with all its visible states (loading,
empty, error, disabled, variants). This page is used for visual verification. Do
not create it if the project does not have one and it was not requested.

## Tests And Storybook

- Add or adapt a component test if the project uses Jest/Vitest and Testing Library.
- Test observable behavior and accessibility, not internal implementation.
- Use `userEvent` for interactions.
- Add a story only if Storybook is already installed and used.

## Do Not

- Do not add a new UI library without confirmation.
- Do not create a design parallel to the design system.
- Do not add `"use client"` to an entire tree.
- Do not ignore loading, error, disabled, or accessibility when the component is interactive.
- Do not modify neighboring components without a direct link.

## Output Format

Summarize:

- **Component created or modified**
- **Prop API**
- **States covered**
- **Accessibility and focus**
- **Tests/Storybook**
- **Limits or decisions to validate**
