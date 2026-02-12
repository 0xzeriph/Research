---
name: research
description: Research any topic — produces concise summary with optional dependency diagram, validates claims, and appends to target file
user-invocable: true
allowed-tools: Read, Write, Grep, Glob, WebSearch, WebFetch, Task
argument-hint: "<topic> [--sources <urls>] [--file <path>]"
---

# Research

When invoked, perform a structured research workflow on the given topic.

## Arguments
- **topic** (required): The subject to research (ERC, EIP, protocol, concept, anything)
- **--sources**: Optional URLs or file paths to use as primary sources
- **--file**: Target file path to append results (default: outputs/research.md)

## Workflow

### Step 1: Research
Launch the **researcher** agent with the topic and optional sources.
The agent will return a concise summary + diagram (if needed) + source links.

### Step 2: Validate
Launch the **validator** agent with the researcher's output and the original topic.
The agent will independently cross-check every claim and return verification flags.

### Step 3: Output
APPEND (never overwrite) to the target file using this format:

```markdown
---
## {Topic}
_Researched: {date}_

{1-2 paragraph summary}

{Mermaid diagram if dependencies exist}

**Validation:** ✅ X verified, ⚠️ Y imprecise, ❌ Z incorrect

**Sources:**
- [source links]
---
```

## Rules
- Be concise. As short as possible, as long as necessary.
- Lead with the key insight — do not bury it.
- Skip preamble and filler.
- APPEND to target file — never overwrite existing content.
- Diagram ONLY shows dependency names, not explanations.
