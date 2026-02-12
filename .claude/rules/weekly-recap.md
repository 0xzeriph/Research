# Weekly Recap Rules

These rules apply when generating weekly recaps.

## Format
- Ultra-concise bullet points. One line per item. No fluff.
- Format: `- [Action verb] [what] -> [file/link if applicable]`
- Write recap bullet points in **British English**.
- Group by day only if there's enough activity to justify it; otherwise list everything flat.

## Process
- Ask the user which week they want (e.g., "week-1"), or infer from context.
- Scan all files in the specified `week-N/` folder and subfolders.
- Read file contents to understand what they cover — do not just list file names.
- If a `week-N/CLAUDE.md` exists, use it for additional context.
- If Notion is connected, also query recently modified pages for that period.
- If the folder is empty or has no modified files, say so clearly.
