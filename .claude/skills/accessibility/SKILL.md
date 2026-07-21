---
name: accessibility
description: "Audits or improves the accessibility of a React/Next.js interface. Covers semantic HTML, keyboard, focus, labels, forms, modals, menus, contrast, text alternatives, dynamic announcements, responsive/zoom behavior, automated tests, and manual verification."
---

# React / Next.js Accessibility

## Scope

Analyze a diff, page, component, or user journey. If no scope is given, ask for
the page or component to audit.

## Severities

- **Blocking**: impossible keyboard journey, field without a name, lost focus, inaccessible action, critical content not announced.
- **Important**: degraded screen reader experience, insufficient contrast, poorly associated form error.
- **Suggestion**: comfort improvement, clearer label, more robust structure.

## Analysis Points

- Semantic HTML: buttons, links, headings, lists, landmarks.
- Keyboard navigation: tab order, Enter/Space activation, Escape, shortcuts.
- Focus: visible, initial, return after close, focus traps.
- Labels and descriptions: accessible name, `aria-describedby`, contextual help.
- Forms: associated errors, error summary, `autoComplete`, required.
- Modals and menus: appropriate role, focus trap, close behavior, open/closed state.
- Contrast: text, informative icons, focus ring, disabled states.
- Text alternatives: images, icons, loaders, charts.
- Dynamic announcements: loading, success, errors, content changes.
- Responsive and zoom: 200%, mobile, orientation, non-truncated text.

## Tests And Checks

- Use Testing Library with accessible queries.
- Run axe, jest-axe, Storybook a11y, or Playwright a11y only if present.
- Plan a manual keyboard and screen reader check for complex patterns.
- Do not stop at an automated score: verify the real journey.

## Do Not

- Do not add ARIA to compensate for the wrong HTML element when a native element fits.
- Do not remove visible outline/focus.
- Do not hide an error only visually.
- Do not make a `div` clickable without role, tabIndex, and keyboard handling.
- Do not introduce an a11y library without confirmation.

## Output Format

Present findings by severity:

- **File and line**
- **Severity**
- **Evidence**
- **User impact**
- **Proposed fix**
- **Recommended test or check**
