# repo-scaffold

> Set up a new project with public/private repo separation and clean AI configuration.

## What This Is

A [Claude Code](https://claude.com/claude-code) skill that scaffolds new projects with a dual-repo pattern: a public repo for code and a private backstage repo for design docs, plans, and research. Includes proper CLAUDE.md/CLAUDE.local.md setup, gitignore configuration, and GitHub repo creation.

## Install

### Claude Code

```bash
# Clone the repo
git clone https://github.com/motiful/repo-scaffold ~/motifpool/repo-scaffold

# Register as a global skill
ln -s ~/motifpool/repo-scaffold/skill ~/.claude/skills/repo-scaffold
```

### Other AI Tools

The skill's knowledge is in `skill/SKILL.md`. You can adapt it for your tool's format (e.g., append relevant sections to AGENTS.md).

## The Pattern

```
<workspace>/
├── <project>/                # Public repo
│   ├── CLAUDE.md             # Public-facing, constitutional
│   ├── CLAUDE.local.md       # Local dev context (gitignored)
│   └── .claude/progress.md   # Execution state (gitignored)
└── <project>-backstage/      # Private repo
    ├── design/               # Design truth
    ├── plan/                 # Execution plans
    └── research/             # Technical references
```

## Usage

In Claude Code, say any of:
- "Scaffold a new project"
- "Create a new repo with the dual-repo pattern"
- "Set up a workspace for my new project"

## License

MIT
