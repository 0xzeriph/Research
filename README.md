# Research Workspace

A Claude Code-powered research assistant. Create structured research tasks, push them to Linear, get weekly recaps, and run automated research with AI agents.

> **Always verify AI-generated research with your own critical thinking and primary sources. AI can make mistakes, hallucinate facts, and miss nuance. Treat every output as a draft that requires human review — not as a final source of truth.**

## Quick Start

1. Clone this repo.
2. Open it with Claude Code (`claude` in your terminal from this directory).
3. Set up Linear integration (see [Integration Setup](#integration-setup)).
4. Start working.

## How It Works

### Directory Structure

Skills and agents live in `.claude/` (your toolbox, available everywhere). Research outputs go in week folders.

```
research-workspace/
├── CLAUDE.md                      <- Always loaded. Workflows, integrations, rules.
├── RESEARCH-TEMPLATE.md           <- Research task template.
├── README.md                      <- You're reading this.
├── .claude/
│   ├── skills/
│   │   └── research/
│   │       └── SKILL.md           <- /research skill — orchestrates agents
│   ├── agents/
│   │   ├── researcher.md          <- Investigates topics, produces summaries
│   │   └── validator.md           <- Cross-checks claims against sources
│   └── rules/
│       ├── research-docs.md       <- Rules for docs in week-*/ folders
│       ├── linear-tasks.md        <- Rules for creating Linear tasks
│       └── weekly-recap.md        <- Rules for weekly recaps
├── personal-rules-example/
│   └── research-style.md          <- Copy to ~/.claude/rules/ for personal preferences
├── week-1/
│   ├── CLAUDE.md                  <- Week-specific context (you create this)
│   └── ...research files...
├── week-2/
│   └── ...
```

### The CLAUDE.md Hierarchy

| Layer | When it's loaded |
|-------|-----------------|
| `CLAUDE.md` (root) | Always |
| `.claude/rules/*.md` (no globs) | Always |
| `.claude/rules/*.md` (with globs) | Only when working on matching files |
| `week-N/CLAUDE.md` | When working inside that folder |

They **stack** — deeper files add specificity, they never replace the root.

## Workflows

### 1. Automated Research (`/research`)

Run the research pipeline with AI agents:

```
/research ERC-4337
/research ERC-4337 --sources https://eips.ethereum.org/EIPS/eip-4337
/research "Rust ownership" --file week-2/rust-notes.md
```

What happens:
1. **Researcher agent** investigates the topic, produces a concise summary + Mermaid diagram (if dependencies exist)
2. **Validator agent** independently cross-checks every claim against official sources
3. Results are **appended** to the target file (never overwritten)

### 2. Create a Research Task

Describe what you need to research. Claude will:
1. Ask clarifying questions.
2. Fill in `RESEARCH-TEMPLATE.md`.
3. Show you the completed template for review.
4. Ask for Linear metadata (team, assignee, due date, etc.).
5. Create the issue in Linear.

### 3. Weekly Recap

```
What did I work on in week-1?
```

Claude scans all files in that week's folder and produces a concise summary.

## Integration Setup

### Linear (Required)

```bash
claude mcp add --transport http linear-server https://mcp.linear.app/mcp
```

Restart Claude Code, then run `/mcp` to complete OAuth.

### Notion (Optional)

1. Create an internal integration at https://www.notion.so/profile/integrations
2. Copy the token (starts with `ntn_`).
3. Share relevant Notion pages with the integration.
4. Run:
   ```bash
   claude mcp add notion-mcp -e NOTION_TOKEN=ntn_YOUR_TOKEN -- npx -y @notionhq/notion-mcp-server
   ```
5. Restart Claude Code.

## Personal Rules

Customize how Claude responds to you without affecting team rules:

1. `mkdir -p ~/.claude/rules`
2. `cp personal-rules-example/research-style.md ~/.claude/rules/research-style.md`
3. Edit to match your style.

Personal rules **add to** team rules — they never override them.

## Creating a New Week

1. Create a folder: `week-N/`
2. Optionally add a `CLAUDE.md` with your weekly focus (even 2-3 lines help).
3. Drop research files as you work.

## File Naming

- Week folders: `week-1`, `week-2`, etc.
- Research files: `kebab-case.md` (e.g., `bridge-security.md`)
