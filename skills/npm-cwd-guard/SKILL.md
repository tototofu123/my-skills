---
name: npm-cwd-guard
description: Enforce correct working directory for package and frontend runtime commands. Use this skill whenever the user asks to run npm, pnpm, yarn, bun, npx, node, vite, or turbo commands, especially when prompts are short like "npm run dev" and directory context must be inferred from active file or recent prompt references.
version: 1.0.0
---

# npm CWD Guard

Use this skill to prevent running package-manager and frontend runtime commands in the wrong folder.

## Command Coverage

Apply this policy to:
- npm
- pnpm
- yarn
- bun
- npx
- node
- vite
- turbo

## Core Rule

Always verify execution directory before running covered commands.

## Resolution Algorithm

Resolve target directory using this strict order:
1. Active file path in editor.
2. Explicit file or folder path mentioned in recent user prompts.
3. Ask user to choose target project folder.

From the selected context path, walk upward and choose the nearest folder that contains package.json.

## Safety Policy

- Never run covered commands from workspace root unless user explicitly requests root execution.
- Never run from a multi-project container folder when a deeper app folder with package.json exists.
- Stay inside current workspace boundaries.

## Past Prompt Inference

When user sends short command-only prompts such as "npm run dev", infer intent from recent prompt context first.

Examples:
- If user recently mentioned a specific project folder, resolve inside that project.
- If user recently referenced a specific file path, resolve from that file and walk upward to nearest package.json.

If inference confidence is low, ask one quick clarification question before running.

## Confirmation Policy

If directory was inferred rather than explicitly provided, provide a short pre-run summary:
- chosen directory
- detected package.json path
- command to run
- reason for folder choice

If user explicitly gave a target path, no extra confirmation is required.

## Missing Context Behavior

If there is no active file and no reliable path in recent prompts:
1. Ask user to pick project folder.
2. Do not guess workspace root.
3. Do not run covered commands until folder is confirmed.

## Output Format

Before run (inferred path case):
- Target folder: <path>
- package.json: <path>
- Command: <command>
- Reason: <one sentence>

After run:
- Working directory used
- Command executed
- Result summary

## Quick Eval Prompts

1. "run dev for BOIDS website make"
Expected: resolve to that project folder, not workspace root.

2. "install deps for doing-projects/efffecthub-v2/src/main.ts"
Expected: resolve by file path and run in nearest package.json folder.

3. "npm run dev"
Expected: infer from recent prompt context; if unclear, ask for target folder.

## Mandatory Activation

Trigger this skill whenever:
1. User asks to run npm, pnpm, yarn, bun, npx, node, vite, or turbo commands
2. Active file context exists that could determine target directory
3. Recent prompts reference specific project folders or file paths
4. Command execution directory is ambiguous in a multi-project workspace

Do not assume workspace root is the target. Always resolve to the nearest package.json relative to the active file or explicitly mentioned path.

## Failure Modes

1. **Wrong Directory**: Running dev/build commands in workspace root instead of project subfolder
   - Fix: Identify active file path, walk upward to nearest package.json, run in that folder

2. **No package.json Found**: Active file is in a folder without package.json
   - Fix: Walk upward to find ancestor folder with package.json, or ask user for explicit project folder

3. **Short Command With No Context**: User types "npm run dev" with no recent file/path references
   - Fix: Ask one quick question: "Which project folder should I run this in?"

4. **Explicit Override**: User says "run this from workspace root"
   - Fix: Honor the explicit request even though it violates safety policy; confirm pre-run

## Success Criteria

Skill is working correctly when:
- npm/yarn/pnpm/bun commands run in the correct subfolder with package.json
- Workspace root is never used unless explicitly requested
- Short prompts infer directory from recent context without guessing
- Pre-run confirmation shows chosen directory, package.json path, and reason
- No "npm ERR! code ENOENT" or similar path-related errors occur
