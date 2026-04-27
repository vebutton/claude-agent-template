# [Project Name] — Bootstrap

> This is the **bootstrap** CLAUDE.md. On first run, Claude Code replaces this file
> with a populated version based on `CLAUDE.md.template`. After bootstrap, this
> file becomes the project's persistent context — loaded automatically at the start
> of every Claude Code session.

## File Roles (read this first)

This repo uses three persistent-context files. Know which is which:

- **`CLAUDE.md`** (this file, after bootstrap) — **project context**. Who the user is,
  what the project does, what it consumes and produces, where things live. Loads every session.
- **`prompts/system_prompt.md`** *(agent projects only)* — **agent behavior rules**. How the
  agent speaks, what it refuses to do, tone, output format discipline, session flow. Apps (CLI
  tools, scripts, web services) skip this — see bootstrap checklist step 3.
- **`collateral/<conversation>.md`** — **one-time input**. The conversation that
  defined this project. Read during bootstrap, then not again. Gitignored by default.

Rule of thumb: if it's *about the project*, it goes in CLAUDE.md. If it's *about how
the agent behaves*, it goes in the system prompt. If it's *why we built this*, it
lived in the conversation and has now been distilled into the two above.

## Workflow Reference

See [project-bootstrap-process.md](project-bootstrap-process.md) for the process
used to kick off this project from a conversation.

## Conversation Reference

See [collateral/](collateral/) for the conversation that defines this project's
requirements. Filename varies per project.

---

## Bootstrap Checklist

Work through these steps **in order** on first run. Do not start building the
agent until all steps are complete.

- [ ] **1. Read the workflow reference** — `project-bootstrap-process.md`. Understand the overall pattern before touching anything.
- [ ] **2. Find and read the conversation file** in `collateral/`. There should be exactly one `.md` file that reads like a conversation or brief. If there are several, ask the user which one defines this project.
- [ ] **3. Infer project type** — agent or app — from the conversation. **Agents** have a defined persona, refuse certain inputs, and produce structured language output (drafts, classifications, summaries) — they need a system prompt. **Apps** (CLI tools, scripts, web services, data pipelines) execute logic and don't need a persona. If the conversation is genuinely ambiguous, ask the user.
- [ ] **4. Read `CLAUDE.md.template`** to see the target structure for the populated CLAUDE.md.
- [ ] **5. (Agent projects only) Read `prompts/system_prompt.md`** to see the target structure for the agent's behavior rules.
- [ ] **6. Rewrite this `CLAUDE.md`** using `CLAUDE.md.template`, populated from the conversation. Project name, user role, project description, inputs, outputs, interface, integrations. Keep it concise — this file loads every session. For app projects, delete the system prompt reference line in the populated file.
- [ ] **7. (Agent projects) Write `prompts/system_prompt.md`** populated from the conversation. This is where behavior rules live: tone, what the agent refuses, output format discipline, session flow, domain facts the agent must know. **Do not duplicate** content between CLAUDE.md and the system prompt — CLAUDE.md is context, system prompt is behavior. **(App projects)** `rm -rf prompts/` instead.
- [ ] **8. Write `docs/requirements.md`** capturing the structured requirements: input formats, output formats, file naming conventions, any hard constraints ("refuse to produce output without X"), integrations needed. This is the spec the agent or app is built against.
- [ ] **9. (Python projects) Set up Python tooling** — use **pyenv** (interpreter) + **uv** (env/deps). Create `pyproject.toml` and `.python-version`, run `uv sync` to create the virtual env, and commit `uv.lock`. Do NOT use venv directly, Poetry, Conda, or Pipenv. The template intentionally does not ship `pyproject.toml` so each project picks its own Python version and dependencies.
- [ ] **10. Check for required collateral** named in the conversation — templates, reference documents, sample data, anything the agent needs to function. If anything required is missing, **stop and ask the user** before proceeding.
- [ ] **11. Fetch/read any external references** the conversation points to (URLs, sample outputs, style references). Note observations in `docs/requirements.md` — do not copy content, just note shape, depth, conventions.
- [ ] **12. Tell the user bootstrap is complete** and summarize: project name, project type (agent/app), what the agent or app does, what you wrote to which files, what (if anything) is still missing. Then wait for direction — do not start the project's real work without explicit go-ahead.

## Post-Bootstrap Process

After bootstrap is complete, the checklist above becomes historical. The user
should replace this entire "Bootstrap Checklist" and "Post-Bootstrap Process"
section with project-specific status tracking from `CLAUDE.md.template`'s
Project Status section.

## Principles for Bootstrap

- **Don't invent scope.** If the conversation doesn't say it, don't assume it.
  Ask the user.
- **Distill, don't transcribe.** CLAUDE.md and the system prompt are not copies
  of the conversation — they're the sharpened extract.
- **Gitignore what's one-time.** The conversation file in `collateral/` stays
  local; the populated CLAUDE.md, system prompt, and docs are what get committed.
- **Ambiguity is a stop-and-ask signal.** When the conversation is unclear on
  something material (output format, integrations, constraints), surface it
  rather than picking a default silently.
