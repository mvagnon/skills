# Skills

Reusable skills repository for Codex and development agents.

## Overview

This repository groups specialized instructions in `skills/`. Each skill is a
directory containing a `SKILL.md` file with:

- `name` and `description` frontmatter;
- usage rules;
- a workflow or standards for a specific type of task.

## Structure

```txt
skills/
  frontend-dev/
    SKILL.md
  write-agent-instructions/
    SKILL.md
  implement-design-system/
    SKILL.md
  write-tests/
    SKILL.md
```

## Available Skills

| Skill | Main use |
| --- | --- |
| `frontend-dev` | Build, refactor, or review frontend API consumption, design-system reuse, component structure, loading states, and UI error handling. |
| `write-agent-instructions` | Generate or update repo-specific `AGENTS.md` files by identifying and rationalizing the active architecture. |
| `implement-design-system` | Apply a Figma or existing design system to an application or monorepo. |
| `write-tests` | Write or repair tests focused on user journeys and business workflows. |

## Add Or Update A Skill

1. Create or update `skills/<skill-name>/SKILL.md`.
2. Keep `name` aligned with the directory name.
3. Write a short, explicit `description` that is optimized for triggering.
4. Document the rules, workflow, and verification criteria.
5. Avoid redundant instructions across skills when a shared rule can be
   referenced or written once.

## Verification

This repository currently has no dependencies, build scripts, or test suite. To
verify a change, review the updated skill and check that its frontmatter remains
valid.
