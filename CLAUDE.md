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
- [ ] **2. Find and read the conversation source** in `collateral/`. This may be one self-contained `.md`, OR a short pointer `.md` plus correlated emails, decks, or PDFs in the same folder. Read everything until you have a coherent picture of what the user wants. If several files could each be "the" conversation, ask the user which one defines this project.
  - **Check the file is text-shaped before reading it whole.** claude.ai's "Copy as GFM" export is HTML-laden despite the `.md` extension — inline `style="..."` on every `<div>`, and a direct `Read` can blow the token limit on the first chunk. If the file looks like HTML, or the collateral includes `.pptx` / `.eml`, extract the text first — see the collateral-format cheatsheet in `project-bootstrap-process.md`.
  - **Verify the conversation reaches a clean handoff** — assistant summarizing, user confirming — not a mid-thread cut. Exports are often truncated. If it ends mid-decision, list what is missing and **ask the user** rather than inventing the gap.
  - **Ask what has changed since.** Conversations capture a moment; by bootstrap time the user has usually thought of more. Ask explicitly: *"Has anything been added or changed since this conversation?"* Capture additions before writing any files.
- [ ] **3. Infer project type** — agent or app — from the conversation. **Agents** have a defined persona, refuse certain inputs, and produce structured language output (drafts, classifications, summaries) — they need a system prompt. **Apps** (CLI tools, scripts, web services, data pipelines) execute logic and don't need a persona. If the conversation is genuinely ambiguous, ask the user.
- [ ] **4. Read `CLAUDE.md.template`** to see the target structure for the populated CLAUDE.md.
- [ ] **5. (Agent projects only) Read `prompts/system_prompt.md`** to see the target structure for the agent's behavior rules.
- [ ] **6. Rewrite this `CLAUDE.md`** using `CLAUDE.md.template`, populated from the conversation. Project name, user role, project description, inputs, outputs, interface, integrations. Keep it concise — this file loads every session. For app projects, delete the system prompt reference line in the populated file.
  - **Route sensitive specifics to `CLAUDE.local.md`** (already gitignored — create it if absent). This covers: employer or company names, internal and unreleased product/feature names, real end-user names and locations, cluster and host names, credential and token workflows, customer or demo dates. Use headed sections — `## Environment / clusters`, `## Credentials & token workflows`, `## Internal context`, `## Dates`, `## Real names / redaction mapping`. Committed files then refer to these generically ("the employer", "the primary cluster", "the end user"). Add a pointer line to the populated CLAUDE.md: *"Internal details: `CLAUDE.local.md` — gitignored, read it every session."*
  - **Write Session State and Project Status as if bootstrap never happened.** They are forward-looking *project* content. No "Bootstrap complete", no "populated CLAUDE.md from the conversation", no "step 13 awaits go" — the first real session needs project state, not bootstrap archaeology.
- [ ] **7. (Agent projects) Write `prompts/system_prompt.md`** populated from the conversation. This is where behavior rules live: tone, what the agent refuses, output format discipline, session flow, domain facts the agent must know. **Do not duplicate** content between CLAUDE.md and the system prompt — CLAUDE.md is context, system prompt is behavior. **(App projects)** `rm -rf prompts/` instead.
- [ ] **8. Write `docs/requirements.md`** capturing the structured requirements: input formats, output formats, file naming conventions, any hard constraints ("refuse to produce output without X"), integrations needed. This is the spec the agent or app is built against. **Apply the same sensitive-specifics routing as step 6** — requirements docs are where cluster names, credentials, real customer names, and internal feature detail leak in most often.
- [ ] **9. Decide the tooling setup** — this branches, like step 3 does. Do not default to Python.
  - **Stack known and Python?** Use **pyenv** (interpreter) + **uv** (env/deps). Create `pyproject.toml` and `.python-version`, run `uv sync` to create the virtual env, and commit `uv.lock`. Do NOT use venv directly, Poetry, Conda, or Pipenv. The template intentionally ships no `pyproject.toml` so each project picks its own Python version and dependencies.
  - **Stack known and non-Python?** Set up that stack's equivalent and record the choice in the populated CLAUDE.md under Tech / Tooling.
  - **Stack not yet decided?** (viability-research phase, or the target is web/mobile and still open) Record the deferral explicitly in the populated CLAUDE.md under Tech / Tooling — what is undecided and what would settle it — then skip the setup.
- [ ] **10. Check for required collateral** named in the conversation — templates, reference documents, sample data, anything the agent needs to function. If anything required is missing, **stop and ask the user** before proceeding.
- [ ] **11. Fetch/read any external references** the conversation points to (URLs, sample outputs, style references). Note observations in `docs/requirements.md` — do not copy content, just note shape, depth, conventions.
- [ ] **12. Tell the user bootstrap is complete** and summarize: project name, project type (agent/app), what the agent or app does, what you wrote to which files, what (if anything) is still missing. Then wait for direction — do not start the project's real work without explicit go-ahead.
  - **Ask for the first concrete objective** — distinct from step 2's "what has changed since the conversation?": *"What is the first concrete objective — the thing that has to work first? And is anything else explicitly secondary to it?"* Priority ordering, must-land-vs-nice-to-have, and "goal A must not be built to depend on goal B" rarely surface in a scoping conversation, but they shape `docs/requirements.md` heavily. Fold the answer into the requirements doc before step 13.
- [ ] **13. (Only on the user's explicit "go" / "ship it" / "create the repo") Finalize and ship the repo.** This step is destructive (`rm -rf .git`) and creates a remote artifact — never auto-run.
  - **Pre-flight (do this FIRST, before the destructive `rm -rf .git`)** — surface identity and permissions now, so failures surface *before* history is reset and the commit is made, not after:
    - **Git commit identity** — run `git config user.name` and `git config user.email`; show the user who the first commit will be authored as: *"This commit will be authored as `<name> <email>` — confirm, or set a repo-local identity now."* In a work context (a company name in the path, or a company target org), proactively suggest setting a repo-local identity (`git config user.email <work-email>`) before committing, rather than silently inheriting personal global config.
    - **GitHub auth** — run `gh auth status`; confirm the active account and that it carries `repo` scope.
    - **Org create-permission** (only when the target is an org repo) — `gh api user/memberships/orgs/<org>` and inspect `role`; if it is not `admin`, warn that repo creation needs the org's "members can create repositories" policy enabled, or an admin to provision.
    - **Visibility availability** — `--internal` requires GitHub Enterprise; if the target org is not on Enterprise, surface that before the user picks visibility, not after `gh repo create` fails.
  - **Pre-cleanup** — remove bootstrap-only files whose job ended: `CLAUDE.md.template` (consumed in step 6); `project-bootstrap-process.md` (workflow doc used during bootstrap — origin-history only, not project context); the template's own `README.md` (replaced wholesale by the project README below — delete it, do not edit it in place); `.gitkeep` files in any directory that now has real content (e.g. `src/.gitkeep` once source files exist); any other bootstrap-only files added by future template versions.
  - **Scrub check — run before staging, on every file that will be committed:**
    - **Scaffolding gate (run after `git add`, before `git commit`)** — a file-level check that does not depend on which instruction the session followed:
      ```bash
      git diff --cached --name-only | grep -E '^(project-bootstrap-process\.md|CLAUDE\.md\.template|README\.md)$'
      ```
      `project-bootstrap-process.md` or `CLAUDE.md.template` in that output means pre-cleanup did not run — **stop and remove them.** A `README.md` hit is only correct if it is the *project* README you wrote below, not the template's; verify, don't assume.
    - **Bootstrap traces** — `grep -riE 'bootstrap|template' CLAUDE.md docs/ prompts/ 2>/dev/null`. The populated files must carry no bootstrap or template references. Scrub any hits. Expect legitimate false positives on projects that genuinely use templates (email templates, report templates) — judge each hit, don't blanket-scrub.
    - **Sensitive specifics that survived step 6/8 routing** — employer and internal product names, real end-user names and locations, cluster/host names, credentials and token workflows, customer or demo dates. Anything found moves to `CLAUDE.local.md`; the committed file refers to it generically. **If the repo will be public, surface a ship-as-is / redact / stay-private decision to the user — do not decide this silently.**
  - **Confirm tooling** — whatever step 9 decided. For Python projects, `pyproject.toml`, `.python-version`, and `uv.lock` should already exist; complete now if missing. If the stack was deferred, confirm the deferral is recorded in CLAUDE.md rather than silently forgotten.
  - **Write `README.md`** — every project repo must ship one. Keep it **brief**: one short paragraph on what the project is and who it's for, then a short pointers section linking to detailed docs (`docs/requirements.md`, `CLAUDE.md` for AI-assist context, anything else relevant). Detail lives in those linked files, not in the README. Do NOT mention the bootstrap process, the template, or how the repo originated.
  - **Reset history and stage** — `rm -rf .git && git init && git add .`. Run `git status` to see the staged tree and draft a first-commit message describing *the project* (e.g. "Initial commit: <one-line project description>"). Do NOT reference the template or the bootstrap process.
  - **Stop. Show the user the staged file tree + proposed commit message. Wait for green-light.** Do NOT push without explicit confirmation. Show the message in **full**, not a summary of it — this is the first time the project is stated back concisely enough to react to, and it reliably draws out scope corrections that open-ended questions earlier did not. It is a last-chance scope check, not just message approval. Do not streamline it away.
  - **On confirmation** — `git commit -m "<approved message>"`, then `gh repo create <project-name> <--private|--public> --source . --push`. Confirm visibility (private or public) with the user before issuing the command — the template does not default to either.
  - **Report and harvest** — give the user the repo URL and confirm the first commit is clean. Then prompt: *"Before you start fresh: did anything in the template friction this bootstrap? This is the moment to capture template improvements while context is fresh."* Capture any friction in `collateral/bootstrap-friction.md` (gitignored — stays local for later harvest into the template repo), one entry per item using the structure **What hit** / **Why it's a problem** / **Suggested fix**. Then recommend they exit this session and start a new one in the same directory for project work — the populated `CLAUDE.md` is now the source of truth and bootstrap context is no longer needed.

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
- **Sensitive specifics are a routing decision, not an afterthought.** Distillation
  naturally pulls employer names, internal product detail, real people, cluster names,
  and credential workflows into committed files — that is what steps 6 and 8 do. Route
  them to `CLAUDE.local.md` *at write time*. Catching them at the push gate is rework;
  missing them is a leak.
- **The conversation is the baseline, not the whole spec.** It captured a moment. Ask
  what has been added or changed since before treating it as complete.
- **Bootstrap scaffolding is template-repo property. None of it ships downstream.**
  `project-bootstrap-process.md`, `CLAUDE.md.template`, and the template's own
  `README.md` exist to run the bootstrap and are deleted in step 13 pre-cleanup.
  If any instruction anywhere appears to say otherwise, this rule wins — do not
  adjudicate it mid-ship, and do not guess by precedence.
- **GitHub artifacts are project-facing, not bootstrap-facing.** The README,
  commit messages, PR descriptions, and any other public-facing text in the
  downstream repo describe *the project*, not *how it was bootstrapped*. Do not
  reference the template, the bootstrap process, or that the repo originated from
  a template — the downstream repo stands on its own.
