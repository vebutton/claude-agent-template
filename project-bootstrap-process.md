# Project Bootstrap Workflow

A process for kicking off new agent or app projects from a conversation — voice
(Claude iOS app), text (claude.ai browser), or a mix of the two. The conversation
captures the substance; this workflow turns it into a working repo.

## Steps

1. **Have the conversation (any Claude interface)**
   - **Voice (iOS app):** great for brainstorming while driving, walking, or at lunch.
     Claude asks clarifying questions and helps refine the concept.
   - **Text (claude.ai browser):** great when you already have artifacts (screenshots,
     existing files, URLs) to paste in, or when you want more structured back-and-forth.
   - **Mix:** start voice on the go, continue in the browser when you're at a computer —
     conversations sync across both.

2. **Copy the full conversation**
   - Open [claude.ai](https://claude.ai) in a browser on your computer.
   - Navigate to the conversation (it syncs from iOS if you started there).
   - Select and copy the entire conversation thread.

3. **Paste into a `.md` file**
   - Open an editor (VSCode, Atom, emacs, etc.).
   - Create a new empty `.md` file (e.g., `my-agent-conversation.md`).
   - Paste the full conversation.
   - Pasting into Word first gives cleaner visual formatting, but `.md` is preferred for portability.
   - This file is a working input — it does NOT stay in the repo long-term (gitignored, see step 8).

4. **Clone the template locally**
   - Clone the template repo into a new directory:
     ```bash
     git clone https://github.com/vebutton/claude-agent-template.git my-new-project
     cd my-new-project
     ```
   - This gives you the full scaffold (`.gitignore`, `CLAUDE.md`, `CLAUDE.md.template`,
     `collateral/`, `output/`, `prompts/`, `src/`, `docs/`).

5. **Drop the conversation file into `collateral/`**
   - Move your `.md` conversation file into the cloned repo's `collateral/` folder.
   - Drop in any other collateral the conversation refers to (templates, sample data,
     reference documents) — the conversation should name these explicitly.
   - `collateral/` is gitignored — safe to leave working inputs there.

6. **Open the project in Claude Code (VSCode or CLI)**
   - Open Claude Code in the repo directory.
   - Claude Code reads `CLAUDE.md` automatically — the bootstrap checklist tells it
     to read the conversation, the templates, and self-populate the repo.

7. **Claude Code self-orients from the conversation**
   Claude Code will:
   - Read `CLAUDE.md` (bootstrap checklist)
   - Read the conversation file from `collateral/`
   - Read `CLAUDE.md.template` and `prompts/system_prompt.md` for target structure
   - Rewrite `CLAUDE.md` with real project context populated from the conversation
   - For agent projects: write `prompts/system_prompt.md` with behavior rules.
     For app projects: delete the `prompts/` directory entirely.
   - Write `docs/requirements.md` with structured requirements
   - Stop and ask if any required collateral named in the conversation is missing
   - Report back when bootstrap is complete, before starting any real work

8. **Create a clean repo and make the first commit**
   - After bootstrap is complete, reset git history so the template scaffolding
     doesn't carry into the project:
     ```bash
     rm -rf .git
     git init
     ```
   - Create the remote repo and push the bootstrapped state as the first commit:
     ```bash
     gh repo create my-new-project --private --source . --push
     ```
   - This way the project's git history starts clean with the populated files.

   What to commit:
   - `CLAUDE.md` — project context, loaded automatically by Claude Code every session
   - `prompts/system_prompt.md` — agent behavior rules (agent projects)
   - `docs/` — structured requirements and specs
   - `project-bootstrap-process.md` — this process doc (reusable across projects)
   - Source code as it develops

   What NOT to commit (gitignore):
   - The raw conversation `.md` file — it's a one-time input; keep a copy in email or elsewhere
   - `TODO.md` — local backlog for future projects, not part of the active repo
   - `.env` / secrets

## Why CLAUDE.md Is the Key File

Claude Code automatically reads `CLAUDE.md` at the start of every session — it is
the project's persistent context. Unlike the conversation file (a one-time input),
`CLAUDE.md` travels with the repo and keeps any Claude Code session instantly oriented.
Think of it as the README *for Claude*, not for humans.

For agent projects, `prompts/system_prompt.md` is the companion file — it holds
behavior rules (tone, what the agent refuses, output discipline) while CLAUDE.md
holds project context. Don't duplicate between them.

## Why a Private Repo

- Requirements docs and code should be version-controlled from day one.
- A private GitHub repo gives you backup, history, and the ability to work across machines.
- It keeps each project separate and namespaced correctly.
- Creating the repo after bootstrap means the first commit is the real project,
  not the template scaffolding — clean history from the start.

## Tips

- Give the conversation `.md` file a descriptive name tied to the project.
- You don't need to clean up the conversation — timestamps, filler, etc. are fine; Claude Code can parse it.
- If the conversation was long, just copy the whole thing; more context is better.
- Keep each repo focused on one agent or app — don't mix projects.
- `TODO.md` is a great place to capture related ideas that came up but aren't in scope yet.
- For Python projects, use **pyenv** (interpreter) + **uv** (env/deps). Don't use venv directly,
  Poetry, Conda, or Pipenv. The template doesn't ship a `pyproject.toml` — create one during
  bootstrap so each project picks its own Python version and dependencies.

## Future / Optional Enhancements to This Process

- Automate repo creation + conversation file placement into a single shell script.
- Explore whether Rovo (Atlassian AI) can complement this workflow for Confluence-heavy projects.

## Editors Tested

- Atom: works fine for pasting raw conversation text; no auto-formatting.
- VSCode: preferred for ongoing development.
- Emacs: available on other machines, fine for `.md`.
