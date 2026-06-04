---
name: implement-design-system
description: Implement a Figma design system into the current target application. Use this skill whenever the user provides a Figma design system, component library, UI kit, tokens, typography, colors, variants, or asks to apply a visual design system to an app or monorepo. This skill is especially important when the repo already has a UI package, design tokens, MUI, Tailwind, shadcn/Radix, custom primitives, or multiple apps/packages that must share the same components.
---

# Implement Design System

Implement a Figma design system into the current target application with the least duplication and the highest fit to the existing architecture.

The Figma input is expected to be a design system: components, variants, tokens, typography, colors, spacing, radius, shadows, icons, and states. Do not assume it contains product screens. Do not invent screens to compensate for missing examples.

## Core Workflow

1. Inspect the target repo before changing files.
   - Identify whether it is a monorepo or a single app.
   - Identify the package manager, framework, base UI utility/library, styling system, shared packages, feature packages, app packages, existing UI folder, tokens, themes, variants, icons, and component export patterns.
   - Search for existing components, hooks, utilities, schemas, and token definitions before creating new ones.

2. Inspect the Figma design system.
   - Use the Figma MCP/app tools when a Figma URL is provided, loading any required Figma prerequisite skills before those tool calls.
   - If the user gives notes with the URL, treat them as prioritization and mapping hints.
   - Inventory components, variants, component props, states, text styles, color styles, effects, spacing, radius, borders, icons, imagery, and required assets.
   - Capture enough Figma evidence to avoid guessing: component metadata where available, screenshots for visual comparison, and Code Connect metadata if the file already has mappings.

3. Decide the implementation target.
   - In a monorepo, prefer the existing shared UI package, such as `packages/ui`, unless the repo clearly uses another shared UI boundary.
   - In a single app, prefer the existing dedicated UI/components directory and existing export conventions.
   - If multiple target apps, UI packages, feature packages, or component systems are plausible, ask the user with the concrete paths and a recommended default before implementation.

4. Map Figma to code.
   - Map Figma tokens to the repo's native token mechanism.
   - Map Figma components to existing primitives first, then extend, compose, or refactor.
   - Add new components only when there is no suitable equivalent.
   - Keep business logic out of UI components unless the existing architecture intentionally combines them.

5. Implement in focused batches.
   - Prefer tokens and foundational theme work before component variants.
   - Implement components and variants that are present in Figma, not speculative additions.
   - If the design system is too large for one pass, propose a batch order based on dependencies: tokens, typography, primitives, form controls, navigation, feedback, layout.

## Token Rules

Store design values in dedicated variables using the native system for the detected stack. Components should consume semantic tokens instead of hardcoded raw values.

Always account for:
- colors: primary, secondary, background, surface, foreground, muted, accent, destructive/error, warning, success, border, ring/focus, disabled
- typography: font families, weights, sizes, line heights, letter spacing, headings, body, captions, labels
- spacing: base scale, component gaps, padding, margins
- radius: small, medium, large, full/pill, component-specific radius only when necessary
- borders: widths, colors, focus rings, dividers
- shadows/elevation: semantic levels and component-specific effects
- motion: durations and easing when the repo already has motion tokens or the Figma system defines them

Use semantic token names that match the existing project. Do not rename an established token system just to match Figma labels.

## Stack-Specific Guidance

### Tailwind CSS

- Use the existing Tailwind version and configuration style.
- For Tailwind v4, prefer CSS variables and `@theme` tokens.
- For Tailwind v3, extend `tailwind.config.*` or existing CSS variables according to local conventions.
- Keep component variants in the existing variant utility if one exists, such as `class-variance-authority`, `tailwind-variants`, or local helpers.
- Do not scatter raw color, spacing, radius, or font values through component class strings when a token can represent them.

### MUI

- Implement the design system through MUI's theme, component wrappers, slots, variants, and targeted style overrides.
- Preserve MUI native behavior: ripple, pressed states, focus handling, keyboard navigation, ARIA behavior, disabled behavior, transitions, and composition APIs.
- Do not replace MUI components with plain HTML just to match visuals.
- Do not override entire components when a palette, typography, shape, spacing, variant, slot prop, or small style override is sufficient.
- Prefer custom app components that compose MUI components when the repo already exposes UI wrappers.

### shadcn/Radix

- Keep Radix accessibility and behavior intact.
- Extend existing shadcn components and variants rather than creating parallel primitives.
- Store colors, radius, and shared values in the existing CSS variable theme.
- Add variants through the existing variant helper and export pattern.

### CSS Modules, Vanilla CSS, or Custom UI

- Use existing CSS variables or create a dedicated token layer if no token layer exists.
- Keep variables grouped by semantic purpose.
- Extend existing components and variant patterns before adding new primitives.
- Avoid component-local hardcoding for reusable design system values.

## Missing Resources and Blockers

Before implementation, stop and ask the user to provide or enable anything you cannot reasonably supply yourself, including:
- missing font files or unavailable font licensing details
- private icon libraries, images, illustrations, or brand assets
- inaccessible Figma files, pages, components, styles, or assets
- unclear target app/package when multiple options exist
- conflicting UI foundations, such as both MUI and custom primitives being used for the same component family
- design system components whose behavior cannot be inferred from Figma visuals or metadata

Do not silently substitute a different font, icon set, asset, or component behavior when the missing resource materially affects the design system.

## Implementation Standards

- Reuse existing code before creating new code.
- Do not duplicate business or visual logic.
- Follow existing type, validation, export, naming, and file organization patterns.
- Keep components accessible and preserve library-provided behavior.
- Keep state local and derived where possible.
- Keep implementation scoped to the design system work requested.
- Do not add dependencies without user approval.
- Do not create tests, logs, screenshots, or evaluation files by default unless the user asks or the project already requires updating directly affected tests.

## Pre-Implementation Report

Before editing files in the target repo, report:

- UI foundation detected: Tailwind, MUI, shadcn/Radix, CSS, custom, or mixed
- target location: shared UI package or app UI folder
- token strategy: where colors, typography, spacing, radius, shadows, and motion will live
- component strategy: wrappers, variants, theme overrides, or new components
- Figma coverage: which components/tokens will be implemented in this pass
- blockers: missing resources or access that must be resolved before implementation

If there are no blockers and the target is unambiguous, proceed with the implementation after the report.

## Verification

After implementation, run the relevant existing checks for the changed scope, such as typecheck, lint, unit tests, or build scripts. Do not run browser automation, dev servers, containers, or long-running commands by default.

Self-review the diff before finishing:
- tokens are centralized in the native stack mechanism
- components consume tokens instead of raw repeated values
- variants correspond to Figma variants
- existing UI library behavior is preserved
- no duplicate components or duplicate visual logic were introduced
- exports match existing package or folder conventions
- missing resources were requested instead of guessed

## Final Response

Summarize:

- tokens created or updated
- components and variants implemented
- native UI library behaviors preserved
- files changed
- checks run and any remaining issues
- resources still needed, if any
