# Repo Scaffold

> Set up a new project with public/private repo separation and clean AI configuration.

## What This Is

An [Agent Skills](https://agentskills.io) compatible skill that scaffolds new projects with a dual-repo pattern: a public repo for code and a private backstage repo for design docs, plans, and research. Includes proper project instructions setup, gitignore configuration, and remote repo creation.

## Install

### Claude Code

```bash
# From marketplace (if published)
/skill install motiful/repo-scaffold

# Or manual
git clone https://github.com/motiful/repo-scaffold ~/skills/repo-scaffold
ln -s ~/skills/repo-scaffold/skill ~/.claude/skills/repo-scaffold
```

### Cursor

```bash
git clone https://github.com/motiful/repo-scaffold ~/skills/repo-scaffold
ln -s ~/skills/repo-scaffold/skill ~/.cursor/skills/repo-scaffold
```

### Other Platforms

Clone the repo and symlink/copy `skill/` to your agent's skills directory.

## The Pattern

```
<workspace>/
├── <project>/                # Public repo
│   ├── CLAUDE.md / AGENTS.md # Project instructions (platform-dependent)
│   └── progress.md           # Execution state
└── <project>-backstage/      # Private repo
    ├── design/               # Design truth
    ├── plan/                 # Execution plans
    └── research/             # Technical references
```

## Usage

Say any of:
- "Scaffold a new project"
- "Create a new repo with the dual-repo pattern"
- "Set up a workspace for my new project"

## License

MIT
