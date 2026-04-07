<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/logo-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset=".github/logo-light.svg">
    <img alt="repo-scaffold" src=".github/logo-light.svg" width="440">
  </picture>
</div>

> Stop guessing your project layout. Pick an archetype, get the right repo structure.

<div align="center">

[![License: MIT][license-shield]][license-url]
[![Version][version-shield]][version-url]
[![Agent Skills][skills-shield]][skills-url]

</div>

<div align="center">
  <a href="#the-problem">Why</a> &middot;
  <a href="#quick-start">Quick Start</a> &middot;
  <a href="#install">Install</a>
</div>

<br>

[Agent Skills](https://agentskills.io) compatible — works with Claude Code, Codex, Cursor, Windsurf, GitHub Copilot, and other Agent Skills adopters.

---

## The Problem

You're building a project with AI. You need design documents, research notes, roadmaps, and progress tracking — but where do they go?

If you're open-source, private thinking doesn't belong in the public repo. If you're running a monorepo, you still need clean AI configuration and skill organization. If you have distributable skills, each one needs its own repo for `npx skills add`. Getting this wrong means either exposing private strategy or over-engineering your structure.

## Features

- **Two project archetypes** — public open-source (split repos) or private monorepo, each with a purpose-built directory layout
- **Local folder = GitHub repo name** — backstage uses `<project>-backstage/`, not generic `backstage/`. No mental translation, no name collisions across projects
- **AI-native configuration** — generates CLAUDE.md (public identity), CLAUDE.local.md (gitignored bridge to private docs), and progress tracking out of the box
- **Skill-aware organization** — guides where to place in-repo skills, product skills, rule-skills, and standalone distributable skills
- **Multi-platform support** — generates platform-appropriate files for Claude Code, Cursor, Codex, and OpenClaw

## Quick Start

```text
"Scaffold a new project"          — creates the full workspace with both repos and AI config
"Set up a workspace"              — same as above, asks which archetype fits your needs
```

## Usage

Say any of these to activate the skill:

```text
"Scaffold a project"
"Set up a workspace"
"Create a new repo"
"Set up a dual-repo project"
```

**Example**

> User: "Scaffold a new project called design-playbook"
>
> repo-scaffold will:
> 1. Ask: public open-source or private monorepo?
> 2. Create the directory structure with all boilerplate files
> 3. Initialize git repos and optionally push to GitHub

**Archetype A — Public open-source** creates split repos under a parent folder:

```
design-playbook-project/
├── design-playbook/                # Public code → GitHub public
├── design-playbook-backstage/      # Private docs → GitHub private
└── design-playbook-<skill>/        # Standalone skill repos (if needed)
```

**Archetype B — Private monorepo** keeps everything in one repo:

```
design-playbook/
├── src/
├── docs/design/, docs/plan/        # Internal docs (in-repo)
├── .claude/skills/                 # In-repo skills
└── skills/                         # Product skills for distribution
```

## Install

```bash
npx skills add motiful/repo-scaffold
```

**Manual registration** (clone + symlink):

```bash
git clone https://github.com/motiful/repo-scaffold ~/skills/repo-scaffold

# Register only in roots you actually use.
ln -sfn ~/skills/repo-scaffold ~/.claude/skills/repo-scaffold      # Claude Code
ln -sfn ~/skills/repo-scaffold ~/.agents/skills/repo-scaffold      # Codex
ln -sfn ~/skills/repo-scaffold ~/.copilot/skills/repo-scaffold     # VS Code / GitHub Copilot
ln -sfn ~/skills/repo-scaffold ~/.cursor/skills/repo-scaffold      # Cursor
ln -sfn ~/skills/repo-scaffold ~/.codeium/windsurf/skills/repo-scaffold  # Windsurf
```

---

## What's Inside

```text
SKILL.md              — Skill definition: archetypes, naming, file roles, setup steps
.github/
  logo-light.svg      — Light mode wordmark
  logo-dark.svg       — Dark mode wordmark
```

## License

MIT — See [LICENSE](LICENSE) for details.

---

Crafted with [Readme Craft](https://github.com/motiful/readme-craft)

<!-- Reference-style link definitions -->
[license-shield]: https://img.shields.io/badge/License-MIT-yellow.svg
[license-url]: https://github.com/motiful/repo-scaffold/blob/main/LICENSE
[version-shield]: https://img.shields.io/badge/version-2.0-blue.svg
[version-url]: https://github.com/motiful/repo-scaffold/releases
[skills-shield]: https://img.shields.io/badge/Agent%20Skills-compatible-DA7857?logo=anthropic
[skills-url]: https://agentskills.io
