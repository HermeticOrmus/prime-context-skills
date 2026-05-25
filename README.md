<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_star.gif" alt="Prime Context Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">Prime Context Skills</h1>

<p align="center">
  <em>A Claude Code skill that primes project context at the start of a session — what the project is, how it's structured, and where to work.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/prime-context-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/prime-context-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/prime-context-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/prime-context-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/prime-context-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/prime-context-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

A single `CLAUDE.md` that loads the essential context of a project at the start of a session, so the first real task lands with full orientation instead of a cold start.

## The problem

The assistant begins every session blind to the project. It does not know what the project is, how it is laid out, what stack it runs on, or what work is in flight. The first prompt of a session pays for that gap: the assistant either guesses at structure and gets it wrong, or burns the prompt asking questions that the repository already answers.

This is the cold-start problem. It repeats at every session boundary: a new day, a context clear, a switch between projects. The fix is a consistent priming step that loads the same high-signal context every time, in the same order.

## The priming steps

| Step | Loads |
|---|---|
| **Read the project's own words** | README and the root manifest: purpose, scripts, stack |
| **Map the structure** | Directory tree, where source and tests and config live |
| **Identify the stack and key files** | Language, framework, the files to read first |
| **Summarize the current state** | Recent commits, branch, in-flight work |
| **Produce a compact orientation** | A short model the assistant holds for the session |

Full content: [`CLAUDE.md`](CLAUDE.md). Worked examples: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/prime-context-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same content is packaged as a skill under [`skills/prime-context/`](skills/prime-context/) for `~/.claude/skills/`. See the `SKILL.md` inside for installation.

### As a `/prime` slash command

To invoke priming on demand, save the protocol as a slash command. Copy [`CLAUDE.md`](CLAUDE.md) to `~/.claude/commands/prime.md`, then run `/prime` at the start of any session.

```bash
curl -o ~/.claude/commands/prime.md https://raw.githubusercontent.com/HermeticOrmus/prime-context-skills/main/CLAUDE.md
```

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/prime-context.mdc`](.cursor/rules/prime-context.mdc).

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## Why this exists

Every session starts the same way: the assistant knows the request but not the project. The request lands against an empty model. The first thing that happens is either a wrong guess about where code lives, or a round-trip of questions the repository could have answered.

Priming front-loads that answer. The protocol reads the project's own description, maps its structure, names the stack and the key files, and summarizes the current state from recent git. The output is a compact orientation the assistant carries for the rest of the session: small enough to hold, specific enough to act on.

The priming step does not explore deeply or carry state forward. Those are separate jobs with their own tools:

- For deep, on-demand exploration of an unfamiliar codebase, see [`explore-code-skills`](https://github.com/HermeticOrmus/explore-code-skills).
- For carrying state across sessions instead of rebuilding it each time, see [`session-handoff-skills`](https://github.com/HermeticOrmus/session-handoff-skills).

Priming is the fast, every-session orientation; exploration is the deep dive; handoff is the bridge between sessions.

## The five steps in detail

### 1. Read the project's own words

Read `README.md` and the root manifest first. The project's own description is higher-signal than inference: the declared scripts reveal how it builds, tests, and runs; the dependencies reveal the stack without guessing.

### 2. Map the structure

List the directory tree two to three levels deep, excluding build and dependency directories. Identify where source, tests, and config live, and name the entry points. The map tells you where to look later.

### 3. Identify the stack and key files

State the language, framework, package manager, and test runner. Name the three to five files a new contributor would read first. Naming them now means later prompts reference them by path instead of rediscovering them.

### 4. Summarize the current state

Read the branch, the last five commits, and the working-tree status. Recent git is the difference between the project's intent and its present, and the present is where the next task starts.

### 5. Produce a compact orientation

Collapse the above into a short summary: one line on what the project is, one on the stack, a few on structure and entry points, one or two on current state. Keep it tight. The point is a model you carry, not a document you file.

## See also

- [`explore-code-skills`](https://github.com/HermeticOrmus/explore-code-skills): deep, on-demand exploration of an unfamiliar codebase, beyond the fast priming pass
- [`session-handoff-skills`](https://github.com/HermeticOrmus/session-handoff-skills): carry state across sessions so the next one resumes instead of re-priming
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): how to behave when directing AI codegen
- [`andrej-karpathy-skills`](https://github.com/HermeticOrmus/andrej-karpathy-skills): how Claude should behave when writing code

## Contributing

PRs welcome, especially for additional worked examples in [`EXAMPLES.md`](EXAMPLES.md), translations of the README, and adaptations of `CURSOR.md` for other AI coding tools (Windsurf, Cline, Aider, Continue, etc.).

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.

---

## Part of the Libre Open-Source Stack for Claude Code

This repository is part of a growing family of open-source toolkits for Claude Code.

### Libre suite — comprehensive plugin bundles

- [LibreUIUX-Claude-Code](https://github.com/HermeticOrmus/LibreUIUX-Claude-Code) — UI/UX development (152 agents, 70 plugins, 76 commands, 74 skills)
- [LibreArch-Claude-Code](https://github.com/HermeticOrmus/LibreArch-Claude-Code) — Software architecture and system design
- [LibreCopy-Claude-Code](https://github.com/HermeticOrmus/LibreCopy-Claude-Code) — Technical writing and documentation engineering
- [LibreDevOps-Claude-Code](https://github.com/HermeticOrmus/LibreDevOps-Claude-Code) — DevOps engineering and infrastructure automation
- [LibreEmbed-Claude-Code](https://github.com/HermeticOrmus/LibreEmbed-Claude-Code) — Embedded systems, firmware, and IoT development
- [LibreFinTech-Claude-Code](https://github.com/HermeticOrmus/LibreFinTech-Claude-Code) — Financial technology development
- [LibreGEO-Claude-Code](https://github.com/HermeticOrmus/LibreGEO-Claude-Code) — AI-search optimization (ChatGPT, Perplexity, Gemini, Google AI Overviews)
- [LibreGameDev-Claude-Code](https://github.com/HermeticOrmus/LibreGameDev-Claude-Code) — Game development across Godot, Unity, Unreal
- [LibreMLOps-Claude-Code](https://github.com/HermeticOrmus/LibreMLOps-Claude-Code) — ML engineering and AI operations
- [LibreMobileDev-Claude-Code](https://github.com/HermeticOrmus/LibreMobileDev-Claude-Code) — Mobile app development (Flutter, React Native, native iOS, native Android)
- [LibreSecOps-Claude-Code](https://github.com/HermeticOrmus/LibreSecOps-Claude-Code) — Security operations
- [LibreSessionFlow-Claude-Code](https://github.com/HermeticOrmus/LibreSessionFlow-Claude-Code) — Session lifecycle: handoff, pickup, absorb, explore, close

### Skills mini-repos — single CLAUDE.md drop-ins

- [vibe-engineer-skills](https://github.com/HermeticOrmus/vibe-engineer-skills) — Direct AI codegen well: hypothesis before help, scoped prompts, validate before accepting
- [markdown-discipline-skills](https://github.com/HermeticOrmus/markdown-discipline-skills) — Strip AI-slop from markdown (no em dashes, no marketing fluff)
- [shell-safety-skills](https://github.com/HermeticOrmus/shell-safety-skills) — `set -euo pipefail` discipline plus 15 failure-mode examples
- [commit-standard-skills](https://github.com/HermeticOrmus/commit-standard-skills) — Ormus Commit Standard v1.0 plus commit-msg hook and commitlint
- [unwoke-skills](https://github.com/HermeticOrmus/unwoke-skills) — Strip AI theater (ten sins to eliminate, symmetric engagement)
- [python-conventions-skills](https://github.com/HermeticOrmus/python-conventions-skills) — Modern Python 3.11+ (types, pathlib, async, ruff, mypy, uv)
- [typescript-conventions-skills](https://github.com/HermeticOrmus/typescript-conventions-skills) — TypeScript strict mode, discriminated unions, Result types
- [hermetic-laws-skills](https://github.com/HermeticOrmus/hermetic-laws-skills) — Seven Hermetic Principles applied to engineering
- [riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills) — Research / Innovate / Plan / Execute / Review systematic dev
- [six-day-cycle-skills](https://github.com/HermeticOrmus/six-day-cycle-skills) — Sustainable shipping cadence with mandatory rest
- [token-optimization-skills](https://github.com/HermeticOrmus/token-optimization-skills) — Claude Code token and context optimization
- [osint-skills](https://github.com/HermeticOrmus/osint-skills) — OSINT research methodology (multi-wave investigative spiral)
- [calcinate-skills](https://github.com/HermeticOrmus/calcinate-skills) — Stage 1 of the Magnum Opus (burn project bloat)
- [claude-md-overhaul-skills](https://github.com/HermeticOrmus/claude-md-overhaul-skills) — Audit CLAUDE.md and MEMORY.md against caps
- [session-handoff-skills](https://github.com/HermeticOrmus/session-handoff-skills) — Session handoff and pickup discipline
- [naming-skills](https://github.com/HermeticOrmus/naming-skills) — Product naming methodology (mine the brand's vocabulary)
- [magnum-opus-skills](https://github.com/HermeticOrmus/magnum-opus-skills) — Seven-stage alchemy applied to project transformation
- [mem-search-skills](https://github.com/HermeticOrmus/mem-search-skills) — Search claude-mem cross-session memory: search, filter, fetch
- [hypothesis-debugging-skills](https://github.com/HermeticOrmus/hypothesis-debugging-skills) — Hypothesis-driven debugging: reproduce, isolate, test, fix
- [vibe-proof-skills](https://github.com/HermeticOrmus/vibe-proof-skills) — Security hardening for vibe-coded full-stack apps
- [tdd-skills](https://github.com/HermeticOrmus/tdd-skills) — Test-driven development (Red-Green-Refactor) for JS/TS and Python
- [mars-skills](https://github.com/HermeticOrmus/mars-skills) — Production-readiness audit: the five mortal sins of vibe-coded MVPs
- [git-workflow-skills](https://github.com/HermeticOrmus/git-workflow-skills) — Clean git workflow: branch, atomic commits, reviewable PRs
- [code-review-skills](https://github.com/HermeticOrmus/code-review-skills) — Domain-aware code review: classify the code, then focus
- [explore-code-skills](https://github.com/HermeticOrmus/explore-code-skills) — Understand an unfamiliar codebase fast
- [dx-audit-skills](https://github.com/HermeticOrmus/dx-audit-skills) — Audit developer experience: docs, onboarding, tooling friction
- [setup-env-skills](https://github.com/HermeticOrmus/setup-env-skills) — Set up a project's development environment
- [automate-skills](https://github.com/HermeticOrmus/automate-skills) — Turn repetitive tasks into reliable automation scripts
- [quick-fix-skills](https://github.com/HermeticOrmus/quick-fix-skills) — Fast troubleshooting for common issues
- [auto-docs-skills](https://github.com/HermeticOrmus/auto-docs-skills) — Generate and maintain project documentation
- [learning-skills](https://github.com/HermeticOrmus/learning-skills) — Learn any technology: roadmaps, explanations, practice, cheatsheets, comparisons
- [linux-sysadmin-skills](https://github.com/HermeticOrmus/linux-sysadmin-skills) — Linux system administration: security, performance, diagnostics, monitoring, maintenance

### Template source

- [andrej-karpathy-skills](https://github.com/HermeticOrmus/andrej-karpathy-skills) — the canonical single-file CLAUDE.md pattern (fork of jiayuan_jy's original)

Star the family, not just one — that's how the suite stays coherent.
