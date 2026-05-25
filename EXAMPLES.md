# Examples

Two worked primings. Each runs the protocol against a real project shape and shows the compact orientation it produces: the model the assistant carries into the first real task.

---

## Example 1: a web app

**Scenario**: a session opens on a project the assistant has not seen. The first task will be "add a saved-filters feature to the dashboard," but before that, prime.

### Step 1: read the project's own words

`README.md` describes a customer dashboard for a logistics product. The root `package.json` declares:

```json
{
  "name": "ops-dashboard",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "test": "vitest",
    "lint": "eslint ."
  },
  "dependencies": {
    "next": "15.1.0",
    "react": "19.0.0",
    "@supabase/supabase-js": "2.45.0"
  }
}
```

Scripts say Next.js with Vitest for tests. Dependencies say React 19 and Supabase as the backend.

### Step 2: map the structure

```text
ops-dashboard/
  app/
    dashboard/
    api/
    layout.tsx
  components/
  lib/
    supabase/
  tests/
  next.config.mjs
```

App Router layout (`app/`, not `pages/`). Source under `app/` and `components/`; shared code in `lib/`; tests in `tests/`.

### Step 3: identify the stack and key files

- Stack: TypeScript, Next.js 15 (App Router), React 19, Supabase, Vitest, ESLint.
- Read first: `app/layout.tsx` (root layout), `app/dashboard/page.tsx` (the surface the task touches), `lib/supabase/client.ts` (data access), `next.config.mjs` (build config).

### Step 4: summarize the current state

Branch `feat/dashboard-filters`. Last five commits add a filter bar component and a `filters` query param. Working tree has uncommitted changes in `app/dashboard/page.tsx`. The in-flight work is filtering; the saved-filters task extends it.

### Compact orientation produced

```text
ops-dashboard: customer dashboard for a logistics product.
Stack: TypeScript + Next.js 15 (App Router) + React 19 + Supabase; tests via Vitest, lint via ESLint.
Structure: routes and UI in app/, shared components in components/, data access in lib/supabase/, tests in tests/.
Entry points: app/layout.tsx (root), app/dashboard/page.tsx (the dashboard surface).
Current state: on feat/dashboard-filters; recent commits added a filter bar and a filters query param; app/dashboard/page.tsx has uncommitted edits. Saved-filters builds directly on this in-flight work.
```

The first task now lands with the file paths and the live edge already in hand.

---

## Example 2: a CLI tool

**Scenario**: a session opens on an unfamiliar command-line tool. The first task will be "add a `--json` output flag." Prime first.

### Step 1: read the project's own words

`README.md` describes a CLI that lints commit messages against a standard. The root `pyproject.toml` declares:

```toml
[project]
name = "commitcheck"
dependencies = ["click>=8.1", "rich>=13.0"]

[project.scripts]
commitcheck = "commitcheck.cli:main"

[tool.pytest.ini_options]
testpaths = ["tests"]
```

The `[project.scripts]` entry points straight at the entry function: `commitcheck.cli:main`. Click for the command interface, Rich for output, pytest for tests.

### Step 2: map the structure

```text
commitcheck/
  src/
    commitcheck/
      cli.py
      rules.py
      output.py
  tests/
  pyproject.toml
```

A `src/` layout. The package is `commitcheck`; the command surface is `cli.py`, the rule engine is `rules.py`, the formatting is `output.py`.

### Step 3: identify the stack and key files

- Stack: Python, Click (CLI), Rich (terminal output), pytest.
- Read first: `src/commitcheck/cli.py` (entry point named by the manifest), `src/commitcheck/output.py` (where a `--json` flag would render), `src/commitcheck/rules.py` (what gets reported).

### Step 4: summarize the current state

Branch `main`, clean working tree. Last five commits stabilized the rule set and added a `--strict` flag. No in-flight work; the tree is a clean base for the new flag.

### Compact orientation produced

```text
commitcheck: a CLI that lints commit messages against a standard.
Stack: Python + Click (CLI) + Rich (output); tests via pytest.
Structure: src/ layout, package src/commitcheck/ where cli.py is the command surface, rules.py the rule engine, output.py the formatter.
Entry point: commitcheck.cli:main (declared in pyproject.toml [project.scripts]).
Current state: on main, clean tree; last commits added a --strict flag. A --json flag belongs in output.py, wired through a cli.py option.
```

The orientation already locates where the new flag wires in (`cli.py` for the option, `output.py` for the rendering) before a line is written.

---

## A note on scope

Priming is deliberately shallow. It reads the project's description, the tree, the manifest, and recent git, not every source file. The output is a model small enough to hold for a whole session, not a survey.

When a task needs more than the priming pass loaded (tracing a data flow across many files, understanding an unfamiliar subsystem in depth) that is exploration, not priming. Reach for [`explore-code-skills`](https://github.com/HermeticOrmus/explore-code-skills) at that point. When the goal is to resume a session rather than start one cold, reach for [`session-handoff-skills`](https://github.com/HermeticOrmus/session-handoff-skills) instead.

## Further reading

- [`explore-code-skills`](https://github.com/HermeticOrmus/explore-code-skills): the deep-dive companion for when priming is not enough
- [`session-handoff-skills`](https://github.com/HermeticOrmus/session-handoff-skills): carrying state across sessions instead of re-priming each time
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): directing AI codegen well once the context is loaded
