# CLAUDE.md

A priming protocol for the start of a session. When you open a project you have not seen this session, run this before doing any work. Merge with project-specific instructions as needed.

**The problem it solves**: you begin every session blind to the project. This protocol loads the essential context fast (what the project is, how it is structured, the stack, the key files, the current state) so the first real task lands with full orientation instead of a cold start.

**Tradeoff**: priming costs a few reads up front. For a one-line change in a file you already have open, skip it. For anything that touches structure you do not already hold, run it first.

## 1. Read the project's own words

Start with what the project says about itself.

- Read `README.md` first. It states the purpose, the audience, and usually the run commands.
- Read the root manifest: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile`, or whatever the stack uses.
- Note the declared scripts and dependencies. Scripts reveal how the project is built, tested, and run. Dependencies reveal the stack without guessing.

The project's own description is higher-signal than inference. Read it before forming a theory.

## 2. Map the structure

Build a mental model of the layout before opening source files.

- List the directory tree two to three levels deep, excluding `node_modules`, `.git`, `dist`, `build`, `target`, `venv`, `__pycache__`.
- Identify the top-level shape: where source lives, where tests live, where config lives.
- Name the entry points: `src/index.ts`, `src/main.py`, `cmd/main.go`, `app/`, or the `main` field in the manifest.

A structure map tells you where to look later. Without it, every follow-up question re-derives the layout.

## 3. Identify the stack and the key files

Turn the manifest and tree into a concrete picture.

- State the language, framework, package manager, and test runner.
- Name the three to five files a new contributor would read first: entry point, core module, config, the largest source file by responsibility.
- Note conventions: a `src/` layout vs flat, a monorepo vs single package, TypeScript vs JavaScript, the lint and format setup.

These are the files you will return to. Naming them now means later prompts can reference them by path instead of rediscovering them.

## 4. Summarize the current state

Read recent history so you know where the project actually is, not just what it was designed to be.

- Read the current branch, the last five commits, and the working-tree status.
- Infer the in-flight work from the diff and the branch name. Uncommitted changes are the live edge.
- Note anything that looks half-done: a stubbed function, a TODO, a failing area the commits were circling.

Recent git is the difference between the project's intent and its present. Both matter; the present is where the next task starts.

## 5. Produce a compact orientation

Collapse everything above into a short summary you can hold for the rest of the session.

A good orientation is roughly:

- One line on what the project is.
- One line on the stack.
- Two or three lines on structure and entry points.
- One or two lines on current state and in-flight work.

Keep it tight. The point is a model you carry, not a document you file. If the summary runs long, you loaded detail you did not need, so compress it.

---

## Run this priming when

- You open a project you have not touched this session → prime before the first task
- You switch projects mid-session → re-prime for the new one
- You resume after a long gap or a context clear → prime to rebuild the model
- A task references structure you do not already hold → prime, then act
- The orientation you formed turns out wrong → re-read the source, do not patch the model

---

**Source**: This file packages a session-priming protocol into a portable artifact. For deeper, on-demand exploration of an unfamiliar codebase, see [`explore-code-skills`](https://github.com/HermeticOrmus/explore-code-skills); for carrying state across sessions rather than rebuilding it, see [`session-handoff-skills`](https://github.com/HermeticOrmus/session-handoff-skills).

**License**: MIT. Use it, fork it, merge it into your own.
