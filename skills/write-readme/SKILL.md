---
name: write-readme
description: Use when creating, replacing, auditing, or updating the root README.md of a Git repository. Trigger whenever the user asks to write a README, update setup docs, document commands, list environment variables, or describe project conventions. This skill enforces a root README that starts with a table of contents and contains Summary, Setup & Commands, Environment Variables, and Project Conventions in that order.
---

# Write README

## Purpose

Use this skill to create or update the root `README.md` of a Git repository.

The README is not a dumping ground for every detail in the codebase. It should give a new contributor enough verified information to understand what the repository is, how to run the useful commands, which environment variables exist, and which project conventions are explicitly documented.

## Target File

Resolve the repository root first:

```sh
git rev-parse --show-toplevel
```

Write or update only `<git-root>/README.md` unless the user explicitly asks for nested package README files.

If the current directory is not inside a Git repository, ask one concise question before writing the file.

## Required README Structure

The README must start with the table of contents. Do not put a title, badges, logo, screenshots, or prose before it.

Use this exact top-level structure:

```md
# Table of Contents

- [Summary](#summary)
- [Setup & Commands](#setup--commands)
- [Environment Variables](#environment-variables)
- [Project Conventions](#project-conventions)

## Summary

## Setup & Commands

## Environment Variables

## Project Conventions
```

Keep these four sections in this order. Add lower-level subsections only when they make the README easier to scan.

## Required Workflow

1. Confirm the Git root and target README path.
2. Read the existing root `README.md` if it exists.
3. Read project instruction files in scope, such as `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.cursorrules`, or equivalent files.
4. Inspect setup and command sources before documenting commands:
   - package manifests such as `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Makefile`, `justfile`, or workspace files;
   - task runner files such as `turbo.json`, `nx.json`, `lerna.json`, `pnpm-workspace.yaml`, or CI workflows;
   - existing docs that already explain installation, development, linting, type checking, testing, building, or deployment.
5. Inspect environment variable sources before documenting variables:
   - `.env.example`, `.env.sample`, `.env.template`, or similar files;
   - typed config schemas, runtime config modules, deployment docs, and CI variable names;
   - never read, copy, or expose private `.env` values.
6. Preserve accurate existing README content, remove stale duplication, and rewrite unclear text into the required structure.
7. Review the diff before finishing.

## Section Rules

### Summary

Explain what the repository is for in a short, concrete paragraph.

Include only facts that are visible in the repository or provided by the user:

- main product, package, service, app, or library purpose;
- major stack or runtime;
- monorepo shape only when it is confirmed by workspace files or project instructions.

Do not invent architecture decisions. If architecture matters and is not documented in project-specific instructions, say that no explicit architecture instructions were found.

### Setup & Commands

Document confirmed setup steps and commands.

Prefer a concise table:

```md
| Task | Command | Notes |
| --- | --- | --- |
| Install dependencies | `pnpm install` | Uses `pnpm-lock.yaml`. |
| Lint | `pnpm lint` | Runs the configured linter. |
```

Include only commands that are present in package scripts, task runner files, Makefiles, CI, or explicit project docs.

When a common command is missing, state it plainly instead of guessing:

```md
No test command is currently documented in this repository.
```

Do not add long tutorials. Link to existing docs when deeper setup steps already exist.

### Environment Variables

List required and optional variables with safe descriptions.

Prefer this table:

```md
| Variable | Required | Description |
| --- | --- | --- |
| `DATABASE_URL` | Yes | Database connection string. |
```

Rules:

- Never include secret values, tokens, credentials, private URLs, or real production identifiers.
- Prefer variable names from examples, schemas, config modules, deployment docs, or CI config.
- Use placeholders only when an example is useful, such as `postgres://...`.
- If no variables are found, write: `No required environment variables are currently documented.`
- If variables appear to exist but their purpose is unclear, list the name only when it is safe and mark the description as `Purpose not documented yet.`

### Project Conventions

Document explicit project conventions only.

Good sources:

- project-specific instruction files;
- existing README or docs;
- package scripts and tooling configs;
- user-provided rules for the current task.

Do not infer architectural conventions from the file tree or code style. If the project has no explicit conventions, write:

```md
No project-specific conventions are currently documented.
```

Keep conventions actionable and short:

- package manager;
- formatting or linting expectations;
- naming conventions explicitly documented by the repo;
- testing policy when documented;
- contribution workflow when documented.

## Editing Guidelines

- Keep the README concise enough to scan quickly.
- Prefer relative Markdown links to files inside the repository.
- Keep headings stable so the table of contents links remain valid.
- Do not duplicate the same command or convention in multiple places.
- Do not document generated output, private deployment details, or local-only machine paths.
- Do not change application code while using this skill unless the user explicitly asks for implementation work too.

## When To Ask

Ask one concise question before editing when:

- the current directory is not inside a Git repository;
- multiple Git roots are plausible and the user did not name the target;
- commands or conventions conflict across official project docs;
- the user asks for architecture or conventions but no project-specific instruction source defines them.

## Completion Checklist

Before finishing, verify:

- The target file is the root `README.md` of the intended Git repository.
- The first heading is `# Table of Contents`.
- The README contains `Summary`, `Setup & Commands`, `Environment Variables`, and `Project Conventions` in that order.
- Commands are copied from verified repository sources or explicitly marked unavailable.
- Environment variables are documented without exposing real secret values.
- Project conventions come from explicit sources, not inferred architecture.
- The diff has been reviewed for stale text, duplication, broken anchors, and unclear wording.
