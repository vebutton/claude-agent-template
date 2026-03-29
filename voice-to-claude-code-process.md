# Voice-to-Claude-Code Workflow

A process for kicking off new agent projects starting from a mobile voice conversation.

## Steps

1. **Voice conversation (Claude iOS app)**
   - Use the iOS app's audio/voice mode to talk through the idea naturally
   - Great for brainstorming while driving, walking, or at lunch
   - Claude will ask clarifying questions and help you refine the concept

2. **Copy full conversation (Claude browser app)**
   - Open [claude.ai](https://claude.ai) in a browser on your computer
   - Navigate to the same conversation (it syncs from iOS)
   - Select and copy the entire conversation thread

3. **Paste into a `.md` file**
   - Open an editor (VSCode, Atom, emacs, etc.)
   - Create a new empty `.md` file (e.g., `my-agent-conversation.md`)
   - Paste the full conversation
   - Pasting into Word first gives cleaner visual formatting, but `.md` is preferred for portability
   - This file is a working scratch pad — it does NOT go into the repo (gitignore it or keep it local)

4. **Create the project repo from the template**
   - Use the Claude agent template repo to bootstrap instantly:
     ```bash
     gh repo create my-new-agent \
       --template vebutton/claude-agent-template \
       --private --clone
     ```
   - This gives you the full scaffold (`.gitignore`, `CLAUDE.md`, `CLAUDE.md.template`,
     `collateral/`, `output/`, `prompts/`, `src/`, `docs/`) with a clean git history

5. **Drop the conversation file into `collateral/`**
   - Move your `.md` conversation file into the cloned repo's `collateral/` folder
   - It's already gitignored — safe to leave there as a working reference

6. **Open the project in Claude Code (VSCode or CLI)**
   - Open Claude Code in the repo directory
   - Claude Code reads `CLAUDE.md` automatically — the bootstrap checklist tells it
     to read the conversation and the template, then self-populate `CLAUDE.md`

7. **Claude Code self-orients from the conversation**
   Claude Code will:
   - Read `CLAUDE.md` (bootstrap checklist)
   - Read the conversation file from `collateral/`
   - Read `CLAUDE.md.template` for the target structure
   - Rewrite `CLAUDE.md` with the real project content populated from the conversation
   - Save context to its memory system (`~/.claude/projects/.../memory/`) for future sessions
   - Create a `docs/` folder with structured requirements documents
   - Create a local `TODO.md` for backlog items not in scope for this repo

8. **Commit only what belongs in the repo**
   What to commit:
   - `CLAUDE.md` — project context, loaded automatically by Claude Code every session
   - `docs/` — structured requirements and specs for the active project only
   - `voice-to-claude-code-process.md` — this process doc (reusable across projects)
   - Source code as it develops

   What NOT to commit (gitignore):
   - The raw conversation `.md` file — treat it as a scratch pad; keep a copy in email or elsewhere
   - `TODO.md` — local backlog for future projects, not part of the active repo
   - `.env` / secrets

## Why CLAUDE.md Is the Key File

Claude Code automatically reads `CLAUDE.md` at the start of every session — it is the project's persistent brain. Unlike the conversation file (which is a one-time input), `CLAUDE.md` travels with the repo and keeps any Claude Code session instantly oriented. Think of it as the README *for Claude*, not for humans.

Key things to put in `CLAUDE.md`:
- Who you are and your role (so Claude tailors responses correctly)
- What this project does (one paragraph)
- Links to the requirements docs in `docs/`
- Key integrations and tools (MCP servers, APIs, etc.)

## Why a Private Repo

- Requirements docs and code should be version-controlled from day one
- A private GitHub repo gives you backup, history, and the ability to work across machines
- It keeps the project separate from personal projects and namespaced correctly

## Tips

- Give the conversation `.md` file a descriptive name tied to the project
- You don't need to clean up the conversation — timestamps, filler, etc. are fine; Claude Code can parse it
- If the conversation was long, just copy the whole thing; more context is better
- Keep each repo focused on one agent or app — don't mix projects
- `TODO.md` is a great place to capture related ideas that came up but aren't in scope yet

## Future / Optional Enhancements to This Process

- Automate repo creation + conversation file placement into a single shell script
- Explore whether Rovo (Atlassian AI) can complement this workflow for Confluence-heavy projects

## Editors Tested

- Atom: works fine for pasting raw conversation text; no auto-formatting
- VSCode: preferred for ongoing development
- Emacs: available on other machines, fine for `.md`
