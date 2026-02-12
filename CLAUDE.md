# Research Task Assistant — Wonderland

## About This Workspace

When the user asks "what is this?", "what does this CLAUDE.md do?", or similar, respond with:

> This is your **Research Task workspace** for Wonderland. It helps you:
>
> 1. **Create structured research tasks** using a standard template and push them directly to Linear.
> 2. **Get weekly recaps** of what you worked on (from files in your week folders).
> 3. **Draft and complete research memos** using context from your working files.
> 4. **Run automated research** with `/research <topic>` — investigates, validates, and saves results.
>
> **Available integrations:**
> - **Linear** — create and manage research tasks (run `setup linear` to connect)
> - **Notion** *(optional)* — pull your work history and notes (run `setup notion` to connect)
>
> Just tell me what you need. If an integration isn't connected yet, I'll walk you through it.

Respond in the same language the user writes in.

---

## Personal Rules

This project includes team-wide rules in `.claude/rules/`. You can also set **personal rules** that follow you across all projects.

Your personal rules live at `~/.claude/rules/`. To get started, copy the example:
```
cp personal-rules-example/research-style.md ~/.claude/rules/research-style.md
```

Feel free to edit them to match your research style. Personal rules never override team rules — they add to them.

See `personal-rules-example/` for a starting point.

---

## Do Not

- Invent, fabricate, or speculate data, sources, or findings in research tasks.
- Create issues in Linear without explicit user approval.
- Delete or overwrite files in any `week-*` folder without asking.
- Assume default values for Linear metadata (team, assignee, etc.) — always ask.
- Modify this CLAUDE.md unless the user explicitly requests it.

---

## Workspace Structure

```
research-workspace/
├── CLAUDE.md                      # This file — general instructions (always loaded)
├── RESEARCH-TEMPLATE.md           # Research task template (referenced, not duplicated)
├── README.md                      # Setup & usage guide
├── .claude/
│   ├── skills/
│   │   └── research/
│   │       └── SKILL.md           # /research skill — orchestrates agents
│   ├── agents/
│   │   ├── researcher.md          # Investigates topics, produces summaries
│   │   └── validator.md           # Cross-checks claims against sources
│   └── rules/
│       ├── research-docs.md       # Rules for research docs (glob: week-*/**)
│       ├── linear-tasks.md        # Rules for creating Linear tasks
│       └── weekly-recap.md        # Rules for generating weekly recaps
├── personal-rules-example/
│   └── research-style.md          # Example personal rules (copy to ~/.claude/rules/)
├── week-1/
│   ├── CLAUDE.md                  # Week-specific context (created by the user)
│   └── ...research files...
├── week-2/
│   └── ...
```

### How CLAUDE.md hierarchy works

- **Root CLAUDE.md** (this file) is always loaded. It contains workflows, integrations, and rules.
- **Week CLAUDE.md** (`week-N/CLAUDE.md`) is loaded additionally when working inside that folder. The user creates it to add context about what they're focusing on that week.
- They stack — the week-level CLAUDE.md adds specificity, it does not replace the root.

### File naming conventions

- Week folders: `week-1`, `week-2`, etc.
- Research files inside weeks: `kebab-case.md` (e.g., `bridge-security.md`, `fee-oracle-analysis.md`).
- Keep file names descriptive and short.

---

## Research Pipeline

The `/research` skill orchestrates two agents:

1. **Researcher agent** — investigates the topic, produces a 1-2 paragraph summary with optional Mermaid diagram
2. **Validator agent** — independently cross-checks every claim against official sources

### Usage
```
/research <topic> [--sources <urls>] [--file <path>]
```

### Examples
```
/research ERC-4337
/research ERC-4337 --sources https://eips.ethereum.org/EIPS/eip-4337
/research "Rust ownership model" --file week-2/rust-basics/notes.md
```

Results are **appended** to the target file (never overwritten), so research accumulates over time.

---

## MCP Setup Guide

When the user says "setup linear", "connect linear", "setup notion", "connect notion", or similar, walk them through the relevant setup below. Be conversational — one step at a time, don't dump everything at once.

### Linear (Official Remote MCP)

1. Run this command in the terminal:
   ```
   claude mcp add --transport http linear-server https://mcp.linear.app/mcp
   ```
2. Restart Claude Code.
3. Run `/mcp` inside Claude Code to complete the authentication flow (it will open a browser for OAuth).
4. Done — you can now create, search, and update Linear issues directly.

### Notion (Optional)

1. Go to https://www.notion.so/profile/integrations
2. Create a new **internal integration** and copy the token (starts with `ntn_`).
3. **Important:** In Notion, share the pages/databases you want accessible with the integration.
4. Run this command (replace `ntn_****` with your real token):
   ```
   claude mcp add notion-mcp -e NOTION_TOKEN=ntn_**** -- npx -y @notionhq/notion-mcp-server
   ```
5. Restart Claude Code. Done.

---

## Research Task Workflow

Follow this sequence for every research task:

1. **Gather** — Read user's description, ask only about missing information.
2. **Generate** — Fill in `RESEARCH-TEMPLATE.md`. Present for review.
3. **Metadata** — Collect Linear properties (team, project, assignee, etc.).
4. **Create** — Push to Linear, confirm with user, share link.

Detailed rules for this workflow are in `.claude/rules/linear-tasks.md`.

---

## Weekly Recap

When the user asks "what did I work on this week?", "resumen semanal", "weekly recap", or similar — scan the requested `week-N/` folder and generate a concise summary.

Detailed rules for format and process are in `.claude/rules/weekly-recap.md`.

---

## Writing Guidelines

- **Conversational responses:** Match the user's language.
- **Research content & tasks:** See `.claude/rules/research-docs.md` and `.claude/rules/linear-tasks.md`.
