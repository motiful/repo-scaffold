---
name: repo-scaffold
description: >-
  Scaffold a new project workspace. Supports two archetypes: public open-source
  (split repos with independent skill distribution) and private monorepo
  (unified codebase with optional doc separation). Use when the user says
  "scaffold a project", "set up a workspace", or "create a new repo".
license: MIT
metadata:
  author: motiful
  version: "2.0"
---

# repo-scaffold

Scaffold a new project with appropriate repo structure and AI configuration.

## When to Use

User wants to create a new project, scaffold a repo, or set up a workspace.

## Execution Procedure

```
scaffold(project_name) → workspace_ready

Step 0: Ask archetype
  IF user specifies public/open-source → Archetype A
  IF user specifies private/internal/monorepo → Archetype B
  IF unclear → ask user

Step 1: Create structure
  IF Archetype A → create_split_repos(project_name)
  IF Archetype B → create_monorepo(project_name)

Step 2: Generate AI config files
  → CLAUDE.md (constitutional, public)
  → CLAUDE.local.md (bridge, gitignored)
  → .claude/progress.md (execution state, gitignored)
  → .gitignore

Step 3: Initialize git repos
  IF Archetype A → git init per subfolder, push to separate remotes
  IF Archetype B → git init single repo, push

GATE: all files exist, git repos initialized, .gitignore correct
```

## Archetype A: Public Open-Source (Split Repos)

For projects published as public repos, especially those with skills or components needing independent distribution.

```
<project>-project/                  # Parent folder (NOT a git repo)
├── .gitignore                      # Contains `*` — prevents accidental git init
├── <project>/                      # Public code repo → GitHub public
│   ├── src/
│   ├── CLAUDE.md                   # Public, constitutional
│   ├── CLAUDE.local.md             # Gitignored, points to backstage
│   ├── .claude/
│   │   └── progress.md             # Gitignored execution state
│   ├── .gitignore
│   ├── README.md
│   └── LICENSE
├── <project>-backstage/            # Private repo → GitHub private
│   ├── design/                     # Design truth
│   ├── plan/                       # Execution plans
│   └── research/                   # Technical references
└── <project>-<skill-name>/         # Standalone skill repo (if needed)
    ├── SKILL.md
    ├── references/
    └── README.md
```

### Key Points

- Parent folder is NOT a git repo — just filesystem namespace
- Each subfolder is an independent git repo with its own remote
- Backstage uses full name `<project>-backstage/` (matches GitHub repo name exactly)
- Skills needing independent distribution get their own repos under the parent
- In-repo skills (`.claude/skills/`) stay in the main code repo

### Naming Convention

| Item | Local path | GitHub name |
|------|-----------|-------------|
| Parent folder | `<project>-project/` | — (not a repo) |
| Public code | `<project>-project/<project>/` | `org/<project>` |
| Private backstage | `<project>-project/<project>-backstage/` | `org/<project>-backstage` |
| Standalone skill | `<project>-project/<project>-<skill>/` | `org/<project>-<skill>` |

**Why full name everywhere:** Local folder name = GitHub repo name. No mental translation needed. No name collisions when cloning multiple projects into the same parent.

### Setup

```bash
# 1. Parent folder
mkdir -p <project>-project && cd <project>-project
echo '*' > .gitignore

# 2. Public repo
mkdir <project> && cd <project> && git init
# Create: CLAUDE.md, CLAUDE.local.md, .claude/progress.md, .gitignore, README.md, LICENSE

# 3. Backstage
cd .. && mkdir <project>-backstage && cd <project>-backstage && git init

# 4. Push
cd ../<project>
gh repo create ORG/<project> --public --source=. --push
cd ../<project>-backstage
gh repo create ORG/<project>-backstage --private --source=. --push
```

## Archetype B: Private Monorepo

For internal projects, SaaS products, or teams preferring a unified codebase.

```
<project>/                          # Single monorepo (private)
├── src/                            # Application code
├── packages/                       # Internal packages (if monorepo tooling)
├── CLAUDE.md                       # Project identity
├── CLAUDE.local.md                 # Gitignored, local dev context
├── .claude/
│   ├── progress.md                 # Gitignored execution state
│   └── skills/                     # In-repo skills (project-specific)
│       └── maintenance-rules/
├── docs/                           # Internal docs (in-repo, since private)
│   ├── design/
│   ├── plan/
│   └── research/
├── .gitignore
└── README.md
```

### Key Points

- Single git repo — code, docs, and in-repo skills together
- No backstage needed — the whole repo is private
- Docs live in `docs/` directly (or split to `<project>-backstage/` if team prefers)
- CLAUDE.local.md still gitignored — contains machine-specific paths and local state

### When to Split Docs Out

Even for private projects, separate backstage makes sense when:
- Multiple repos share the same design docs
- Non-engineering stakeholders need doc access without code access
- Docs have a different review/approval workflow

If splitting: create `<project>-backstage/` as a sibling repo.

### Skill Organization (Private Projects)

| Skill type | Location | When to use |
|-----------|----------|-------------|
| In-repo | `.claude/skills/<name>/` | Project-specific rules, workflows, maintenance |
| Product skill | `skills/<name>/SKILL.md` | Skills distributed with the product to end users |
| Rule-skill | `.claude/skills/<name>-rules/` | Constraint packages (MUST/NEVER rules) |
| Collection | `skills/*/SKILL.md` (multi) | Multiple skills always consumed together |
| Personal tool | Graduate to standalone repo | Cross-project utility worth sharing |

In-repo skills use relative symlinks for cross-vendor: `.agents/skills/X → ../../.claude/skills/X`

Product skills at repo root follow collection layout when multiple: `skills/<a>/SKILL.md`, `skills/<b>/SKILL.md`.

For detailed taxonomy and publishing guidance, see skill-forge.

## File Roles

### CLAUDE.md — public, in git

Constitutional: stable, rarely-changing content.
- Project identity, tech stack, design principles, code conventions
- No internal paths, no backstage references, no progress tracking

### CLAUDE.local.md — private, gitignored

CC reads automatically alongside CLAUDE.md. Must live at project root.

Template (Archetype A):
```markdown
# Local Development Context

## Internal Docs
Design documents and plans live in a sibling private repo:
`../<project>-backstage/`

## Execution State
Read `.claude/progress.md` for current phase and next steps.

## Git Workflow
Two repos, two commit flows:
- Code changes → commit here
- Doc changes → commit in `../<project>-backstage/`
```

Template (Archetype B):
```markdown
# Local Development Context

## Execution State
Read `.claude/progress.md` for current phase and next steps.

## Internal Docs
Design docs in `docs/design/`, plans in `docs/plan/`.
```

### .claude/progress.md — private, gitignored

Dynamic execution state: current phase, completed items, next steps. Referenced by CLAUDE.local.md, never by CLAUDE.md.

### .gitignore must include

```
.claude/
CLAUDE.local.md
node_modules/
dist/
*.tsbuildinfo
.DS_Store
.env
coverage/
```

## Platform Support

| Platform | Project instructions | Local bridge | Progress |
|----------|---------------------|-------------|----------|
| Claude Code | `CLAUDE.md` | `CLAUDE.local.md` | `.claude/progress.md` |
| Cursor | `.cursor/rules/*.mdc` | `.cursor/rules/personal.mdc` | `.cursor/progress.md` |
| Codex | `AGENTS.md` | `AGENTS.override.md` | tracked in AGENTS.md |
| OpenClaw | `AGENTS.md` | via `openclaw.json` | tracked in AGENTS.md |

## Daily Workflow

Run CC in the code repo root. CC reads CLAUDE.md + CLAUDE.local.md automatically — full backstage context without special prompts.
