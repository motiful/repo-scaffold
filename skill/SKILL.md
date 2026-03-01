# repo-scaffold

Set up a new project with public/private repo separation and clean AI configuration.

## When to Use

User says things like:
- "新建一个项目"
- "scaffold a new project"
- "帮我搭个项目结构"
- "set up repos for X"

## The Pattern

Every serious project has two kinds of content:
1. **Public** — code, README, LICENSE → GitHub public repo
2. **Private** — design docs, plans, research, debates, drafts → separate private repo

They live as siblings under a workspace directory:

```
~/motifpool/<project>/
├── <project>/                # Public repo (GitHub public)
│   ├── src/
│   ├── CLAUDE.md             # Public-facing, clean, constitutional
│   ├── .claude/
│   │   ├── CLAUDE.local.md   # Private dev context (gitignored)
│   │   └── progress.md       # Execution state (gitignored)
│   ├── .gitignore            # Must include .claude/
│   ├── README.md
│   └── LICENSE
└── <project>-backstage/      # Private repo (GitHub private)
    ├── design/               # Design truth
    ├── plan/                 # Execution plans
    └── research/             # Technical references
```

## Key Rules

### CLAUDE.md (public)
- **Constitutional** — only stable, rarely-changing content
- Project identity, tech stack, design principles, code conventions
- NO internal paths, NO backstage references, NO progress tracking
- This is what the world sees. Keep it clean.

### .claude/CLAUDE.local.md (private, gitignored)
- CC reads this automatically alongside CLAUDE.md
- Contains: backstage path, old project references, git workflow, local dev context
- Template:

```markdown
# Local Development Context

## Internal Docs
Design documents and plans live in a sibling private repo:
`../PROJECT-backstage/`

Read design docs: `Read ../PROJECT-backstage/design/DESIGN_DOC.md`

## Execution State
Read `.claude/progress.md` for current phase and next steps.

## Git Workflow
Two repos, two commit flows:
- Code changes → commit here
- Doc changes → commit in `../PROJECT-backstage/`
```

### .claude/progress.md (private, gitignored)
- Dynamic execution state — current phase, completed items, next steps, pending research
- Referenced by CLAUDE.local.md, never by CLAUDE.md

### .gitignore must include
```
.claude/
node_modules/
dist/
*.tsbuildinfo
.DS_Store
.env
.env.*
coverage/
```

## Setup Steps

1. Create workspace and repos:
```bash
mkdir -p ~/motifpool/<project>
cd ~/motifpool/<project>

# Public repo
mkdir <project> && cd <project>
git init

# Private repo
cd ..
mkdir <project>-backstage && cd <project>-backstage
git init
```

2. Create CLAUDE.md (public) — project identity only
3. Create .claude/CLAUDE.local.md — backstage path and local context
4. Create .claude/progress.md — initial execution state
5. Create .gitignore — include .claude/
6. Push to GitHub:
```bash
cd <project>
gh repo create ORG/<project> --public --source=. --push
cd ../<project>-backstage
gh repo create ORG/<project>-backstage --private --source=. --push
```

7. Update ~/motifpool/.gitignore with both directory names

## Naming Convention

| What | Name |
|------|------|
| Workspace dir | `<project>/` |
| Public repo | `<project>/` (same name) |
| Private repo | `<project>-backstage/` |
| GitHub public | `org/<project>` |
| GitHub private | `org/<project>-backstage` |

## Daily Workflow

Developer runs CC in `<project>/<project>/` (the public repo). CC reads CLAUDE.md + CLAUDE.local.md automatically. To access design docs, CC reads `../PROJECT-backstage/...`. No special prompts needed.
