# Using this repo with Cursor

This project includes a Cursor project rule so the priming protocol is available when you work here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/prime-context.mdc`](.cursor/rules/prime-context.mdc) is committed with `alwaysApply: false`, so it loads on demand rather than on every request.
3. In Cursor, confirm under **Settings → Rules**, where `prime-context` should appear. Reference it at the start of a session to prime the project context.

## Use the same protocol in another project

**Cursor (recommended)**: Copy `.cursor/rules/prime-context.mdc` into that project's `.cursor/rules/` directory (create the folders if needed). Merge with existing rules as you like.

**Other AI coding tools**: If a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead (or merge its contents into your existing instructions). Most modern AI coding tools (Claude Code, Continue, Cline, Windsurf, Aider) read a root-level instruction file.

## Optional: personal Agent Skills

If you want the same content as a reusable skill under `~/.cursor/skills`, use [`skills/prime-context/SKILL.md`](skills/prime-context/SKILL.md). Copy or symlink it into your personal skills directory.
