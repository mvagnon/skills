---
name: write-tests
description: Use when the user asks to write, add, improve, or repair tests for one or more user journeys, product flows, API workflows, business processes, scenarios, funnels, or "parcours". This skill is mandatory when the user invokes write-tests or provides $ARGUMENTS describing journeys to test. It prevents fragile component, file, class, Tailwind, and implementation-detail tests by forcing broad, robust journey coverage.
disable-model-invocation: true
argument-hint: "[user-journeys]"
---

# Write Tests

## Input Contract

This skill takes exactly one argument: `$ARGUMENTS`.

Interpret `$ARGUMENTS` as one or more journeys to test. A journey is a user-visible workflow, API workflow, business process, or product scenario with a beginning, actions, and observable outcomes.

If `$ARGUMENTS` names files, components, hooks, classes, endpoints, or routes, use those names only as discovery anchors. Find the real journey they participate in, then test that journey. Do not test the file, component, hook, class, endpoint, or route in isolation.

If no journey can be inferred, ask one concise question for the journey to cover before writing tests.

## Prime Directive

Never test components or files individually. Test parcours.

The trinity of a successful test:

1. Test a journey, not an element.
2. Make the test robust enough that failures normally mean fixing the product code, not the test.
3. Cover a broad enough scope that the tested journey gives real regression confidence.

## Required Workflow

1. Read project instructions first: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or equivalent files in scope.
2. Inspect the existing test architecture before coding: test directories, package scripts, helpers, fixtures, factories, mocks, setup files, custom render utilities, API clients, and existing journey tests.
3. Search for existing tests around the same flow and extend or reuse them before creating new files.
4. Identify each journey from `$ARGUMENTS`: entry point, actors, data setup, actions, async boundaries, success outcomes, failure outcomes, authorization rules, and side effects.
5. Choose the highest meaningful test level already used by the project:
   - Frontend: route, page, feature, or integration tests that exercise user interactions and observable behavior.
   - Backend: request, use-case, service workflow, or integration tests through public boundaries.
   - Domain logic: test the business scenario, rule, or invariant as a workflow, not private helpers or isolated classes.
6. Reuse existing test utilities and data builders. Do not add dependencies unless the user explicitly approves.
7. Write tests that set up realistic state, perform the journey, and assert stable outcomes.
8. Run the relevant existing test or check command for the changed area. Do not start dev servers, containers, or browser automation by default.

## What To Test

Prefer assertions that prove the journey works:

- A user can complete the flow and sees the expected stable outcome.
- The correct request, command, mutation, or service call is made with the expected parameters.
- Data is created, updated, persisted, returned, or invalidated according to the contract.
- Validation, authorization, success, loading, disabled, empty, and error states are handled when they are part of the journey.
- External boundaries are called correctly and their success/error contracts are handled correctly.
- Navigation, cache updates, emitted events, or side effects happen when they are part of the product contract.

## What Not To Test

Do not write tests that assert:

- TailwindCSS classes, CSS module class names, generated class names, inline style values, spacing tokens, colors, or visual implementation details.
- A component, hook, file, private method, or class works in isolation when the user asked for a parcours.
- Internal state shape, non-public helper calls, implementation-specific mocks, or incidental render structure.
- Exact snapshots for broad UI output unless the project already relies on them and the snapshot represents a stable contract.
- Hardcoded data returned by external services, CMS systems, or translation files.

Avoid empty smoke tests such as "renders component" unless they are part of a larger journey test with meaningful behavior assertions.

## Robust Frontend Assertions

Use user-observable behavior and accessibility semantics when they express stable product contracts:

- Prefer roles, form controls, enabled/disabled state, submitted values, navigation changes, API calls, persisted records, and visible success/error state.
- Use `userEvent` or the project's equivalent user interaction helper instead of calling handlers directly.
- Use `data-testid` only when semantic queries are not practical and the project already treats the test id as a stable journey anchor.
- Do not query by Tailwind classes or DOM structure created by a styling/library implementation.

For translated content, do not hardcode translated copy such as:

```ts
expect(screen.getByText("Bienvenue sur l'app")).toBeInTheDocument();
```

When the i18n layer is mocked, prefer asserting the translation key:

```ts
expect(t).toHaveBeenCalledWith("home.welcomeMessage");
```

When the i18n layer is not mocked, assert stable behavior around the translated content instead of the exact translated string whenever possible.

## External Data And Contracts

Do not hardcode assertions for volatile data from:

- external services;
- CMS systems;
- i18n or translation files;
- third-party SDKs;
- time, currency, locale, feature flag, or environment dependent sources.

Test the contract, reference, call, or schema instead:

- The service was called with expected stable parameters.
- The response satisfies the expected schema or domain contract.
- Success and error paths are handled correctly.
- The UI or API exposes the stable outcome promised by the product.

Only assert an exact payload when the test fully controls that payload and the payload itself is the contract under test.

## Completion Checklist

Before finishing, verify:

- Every new or changed test maps to one named journey from `$ARGUMENTS`.
- No test targets a component, file, class, private helper, or Tailwind class in isolation.
- Existing helpers, factories, mocks, and project test patterns were reused.
- Assertions are stable and behavior-focused.
- Volatile CMS, i18n, external service, time, and environment data is mocked or asserted through contracts.
- Relevant test commands were run, or the reason they could not be run is reported clearly.
