---
name: frontend-design
description: Use when building, refactoring, or reviewing frontend code that consumes APIs, defines custom data hooks, splits UI components, handles loading states, or handles UI errors. This skill guides Codex to follow the existing frontend data-fetching architecture, reuse existing design-system primitives, avoid duplicate components, and implement appropriate spinners, skeletons, and error states.
---

# Frontend Design

## API Consumption

Follow the existing project data-fetching architecture.

When the project uses `TanStack Query` (recommended):

- Frontend components consume APIs through custom hooks.
- Custom hooks use `TanStack Query`.
- Custom hooks do not call `fetch()` directly.
- Raw network calls live in dedicated API/client functions.
- Hooks expose data, loading or pending state, error state, and mutation status when relevant.
- Errors are handled intentionally and never swallowed silently.
- Query keys are stable, explicit, and colocated with the related feature when possible.

If the project uses another data layer, follow that project's existing pattern instead.

## Components

### Core Rules

- Keep each meaningful component in its own file.
- Do not define multiple substantial subcomponents inside one large component file.
- Prefer variants over near-duplicate components.
- Keep mobile-first responsive components and pages.
- Reuse existing UI components: layout, typography, form, button, modal, table, card, navigation, and related primitives.
- Creating a new UI component when one already exists is a bug. Reuse the existing component, extend it, compose it, or create a variant.
- Avoid large component files; prefer multiple smaller files.

Avoid:

```txt
components/tasks-kanban.tsx
```

Prefer:

```txt
components/tasks-kanban/tasks-kanban.tsx
components/tasks-kanban/kanban-column.tsx
components/tasks-kanban/kanban-item.tsx
components/tasks-kanban/tasks-kanban-filters.tsx
```

## Loading States And Error Handling

Loading states are a crucial part of the user experience.

Use loading states when a component is waiting for an async action to display data, such as API consumption or client-side processing. Do not implement loading states for synchronous actions such as rendering a static component.

### Spinners

Use spinners for components that trigger an async action, often pushing data to an external source or sending forms.

You should:

- Avoid text-only loading changes such as changing "Send message" to "Sending...".
- Disable actions that cannot safely run twice.
- Implement an error snackbar when an error occurs.

Good example:

```tsx
<Button onClick={handleConfirm} disabled={isPending}>
  {isPending && <Spinner />}
  Confirmer
</Button>
```

### Skeletons

Use skeletons for components that display data from an async action.

You should:

- Preserve global layout stability during loading.
- Replicate the actual component with less detail.
- Implement an error early return with a retry button when an error occurs.
- Not use skeletons when a spinner is already visible for the same request, such as when a button invalidates data.
- Use skeletons mostly for components at their initial loading state.

Good example:

```tsx
function RevenueCard() {
  const { data, isPending, isFetching, error, refetch } = useRevenue();

  if (isPending) return <RevenueCardSkeleton />;

  if (!!error)
    return (
      <RevenueCardError
        error={error}
        isRetrying={isFetching}
        onRetry={() => refetch()}
      />
    );

  return <RevenueCardContent data={data} />;
}
```

## Completion Checklist

Before finishing, verify:

- Frontend API consumption follows the existing data-fetching architecture.
- Components use existing UI primitives before new ones are created.
- Meaningful components are split into focused files.
- Async actions use disabled states, spinners, and intentional error handling.
- Async data display uses skeletons and retryable error states when relevant.
- No duplicate component, hook, API, validation, or loading-state logic was introduced.
