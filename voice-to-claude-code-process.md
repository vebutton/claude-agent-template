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

4. **Create the project repo**
   - Create a private GitHub repo for the project
   - Initialize git locally and connect it
   - Set up `.gitignore` early — exclude the conversation file, `TODO.md`, secrets, and any other local-only files

5. **Open the project in Claude Code (VSCode or CLI)**
   - Put the `.md` conversation file in your project directory temporarily
   - Open Claude Code in the same workspace
   - Reference the file to give Claude Code the full context to work from

6. **Let Claude Code scaffold the project structure**
   Claude Code will:
   - Read the conversation and extract requirements
   - Save context to its memory system (`~/.claude/projects/.../memory/`) for future sessions
   - Create a `CLAUDE.md` in the project root — this is the primary persistent context file
   - Create a `docs/` folder with structured requirements documents
   - Create a local `TODO.md` for backlog items not in scope for this repo

7. **Commit only what belongs in the repo**
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

- Automate the repo creation step with a shell script or Claude Code hook
- Add a `CLAUDE.md` template to reuse across new projects
- Explore whether Rovo (Atlassian AI) can complement this workflow for Confluence-heavy projects

## Editors Tested

- Atom: works fine for pasting raw conversation text; no auto-formatting
- VSCode: preferred for ongoing development
- Emacs: available on other machines, fine for `.md`
