# Retrofit — Bootstrapping an In-Flight Project

> Instructions for putting an **existing, already-worked-on** Claude Code project onto
> the same footing as one bootstrapped from the template. This file is fetched or copied
> into the target project. It is **scaffolding** — deleted in Phase 4 pre-cleanup, never
> committed, never referenced in the project's README or commits.

## When this applies

Use this when **all** of the following are true:

- A real working directory exists with real work in it — code, outputs, notes.
- It was never bootstrapped: no populated `CLAUDE.md`, no `docs/requirements.md`,
  no session state.
- Its git history is absent or throwaway.

**Do not use this when the directory is a clone or fork whose upstream history must be
kept.** Phase 4 resets history. In that case stop and tell the user — bootstrapping a
fork is a first-action decision, not a retrofit.

## Retrofit is bootstrap inverted

| | Bootstrap | Retrofit |
|---|---|---|
| What exists | A defining conversation | Working code and artifacts |
| What's missing | The code | The conversation |
| Spec source | Distil the conversation | Distil the artifacts, then interview |
| Risk | Inventing scope | Mistaking what was built for what was intended |

The artifacts tell you **what** the project does. Only the user knows **why**, what was
abandoned, and what is load-bearing versus accidental. Never treat the code as the whole
spec — that is the single biggest failure mode of this process.

---

## Phase 0 — Make the project exitable

**Do this first, always, before any other phase.** An unbootstrapped in-flight project
has no session state, which means there is no clean way to end a session and return to
it. Phase 0 fixes that, and it is worth doing on its own: if the session ends right
after Phase 0, the project is recoverable.

- [ ] **0.1 Inventory the directory.** `ls -R` (minus `node_modules`, `.venv`, caches).
      Identify entry points, generated outputs, scratch files, existing docs, config.
      Establish shape — do not read everything yet.
- [ ] **0.2 Ask the user for the orientation paragraph.** *"In a paragraph: what is this
      project, and what has been done so far?"* Do not guess this from the directory name
      or the code. It costs one question and anchors everything downstream.
- [ ] **0.3 Write `docs/session-state.md`** — the archaeology of the work to date, as
      best it can be reconstructed. Record what was built, in what order, and any
      decisions visible in the artifacts. **Mark each entry as confirmed by the user or
      reconstructed by you** — a retrofit's archaeology is partly inference, and the next
      session needs to know which is which.
- [ ] **0.4 Write a minimal `CLAUDE.md`** — project name, the orientation paragraph, and
      a forward-looking Session State brief ending in the standardized pointer line:
      *"Full archaeology: `docs/session-state.md` — consult when prior-thread depth,
      decision reasoning, or ruled-out paths are needed."*
      This is a deliberate stub. Phase 1 replaces it. It exists so the session can end
      safely before the rest of the retrofit runs.
- [ ] **0.5 Tell the user Phase 0 is done** and that they can exit cleanly now — Phases
      1–4 can run in a later session, and `CLAUDE.md` will orient that session correctly.

---

## Phase 1 — Reconstruct the spec

- [ ] **1.1 Infer project type — agent or app.** **Agents** have a defined persona, refuse
      certain inputs, and produce structured language output; they need a system prompt.
      **Apps** (CLI tools, scripts, web services, pipelines) execute logic and need no
      persona. If genuinely ambiguous, ask.
- [ ] **1.2 Read the artifacts properly** — source, outputs, prompt text wherever it
      lives, config, existing docs, `TODO`/scratch files. Build a concrete picture of what
      the project consumes, what it produces, and where each lands.
- [ ] **1.3 Draft your understanding and show it to the user** before writing any file:
      what this is, inputs, outputs, interface, integrations, tech, and any conventions
      you observed. A wrong draft is easier to correct than an open question.
- [ ] **1.4 Interview on what code cannot tell you.** Ask in a batch, not one at a time:
      - What is deliberate versus accidental?
      - What is dead, abandoned, or superseded?
      - What are the hard constraints? (*"never produce output without X"*)
      - Who consumes the output, and in what form?
      - What was tried and ruled out? (invisible in code, expensive to rediscover)
      - What was the first effort's actual goal, and was it met?
- [ ] **1.5 Rewrite `CLAUDE.md` in full** against `CLAUDE.md.template`. If the template is
      not present locally, fetch it:
      ```bash
      gh api -H "Accept: application/vnd.github.raw" \
        repos/vebutton/claude-agent-template/contents/CLAUDE.md.template
      ```
      **Route sensitive specifics to `CLAUDE.local.md` at write time** (create it if
      absent; it is gitignored): employer or company names, internal and unreleased
      product names, real end-user names and locations, cluster and host names,
      credential and token workflows, customer or demo dates. Use headed sections —
      `## Environment / clusters`, `## Credentials & token workflows`,
      `## Internal context`, `## Dates`, `## Real names / redaction mapping`. Committed
      files then refer to these generically. Add the pointer line to `CLAUDE.md`.
- [ ] **1.6 Write `docs/requirements.md`** — input formats, output formats, naming
      conventions, hard constraints, integrations. **Apply the same sensitive-specifics
      routing** — requirements docs are where internal detail leaks in most often.
- [ ] **1.7 (Agent projects) Write `prompts/system_prompt.md`.** In-flight projects
      usually have prompt text scattered through code, config, or notes — **consolidate
      what exists, do not invent a persona.** Keep the boundary: `CLAUDE.md` is project
      context, the system prompt is behavior. **(App projects)** do not create `prompts/`.
- [ ] **1.8 Update `docs/session-state.md`** with anything the interview surfaced —
      especially ruled-out paths, which exist nowhere else.

---

## Phase 2 — Align the scaffolding, not the substance

The rule: **align the scaffolding closely; leave the substance where it is.**

**Align** (add if missing, conform if present):
- `docs/` and `docs/session-state.md`
- `README.md` — project-facing (written properly in Phase 4)
- `.gitignore` — must cover `collateral/`, `output/`, `CLAUDE.local.md`, `.env*`,
  secrets, OS cruft, and `.claude/settings.local.json`
- `collateral/` — one-time inputs, gitignored
- `output/` — generated artifacts, gitignored
- `CLAUDE.local.md`

**Leave in place:**
- Extensive existing code and documentation. Do **not** relocate working code into `src/`
  purely to match the template shape.
- Anything that diverges significantly from an upstream clone or fork it came from.

**Every move of an existing file is a proposal, never silent.** List proposed moves and
get approval. Pay particular attention to working files sitting in the project root that
are really collateral or output — moving them under the gitignored directories is the
cheapest protection against committing them in Phase 4.

---

## Phase 3 — Tooling and tracks

- [ ] **3.1 Record the tooling.** The stack is already chosen de facto; the retrofit's job
      is to make it explicit under **Tech / Tooling** in `CLAUDE.md`. If it is Python and
      not already **pyenv** (interpreter) + **uv** (env/deps), propose regularizing —
      `pyproject.toml`, `.python-version`, committed `uv.lock`. **Ask first:** this is the
      one place a retrofit touches a working setup. Never Poetry, Conda, Pipenv, or bare
      venv. If part of the stack is genuinely undecided, record the deferral explicitly:
      what is open and what would settle it.
- [ ] **3.2 Establish tracks — even when there is only one.** A **track** is a distinct
      workstream with its own objective. An in-flight project reaching retrofit has
      completed at least one.
      - Populate the `## Tracks` section carried by `CLAUDE.md.template`. Record the
        completed work as **Track 1** with its objective and status.
      - Give `docs/requirements.md` per-track sections, so a later track appends instead
        of forcing a rewrite.
      - Ask whether any next tracks are known. ***"None yet" is a valid and expected
        answer*** — record the structure regardless, and do **not** invent tracks to fill
        it. The point is that the project knows more tracks can arrive.
      - Note the rule for future tracks: when one is added, state explicitly whether it
        may depend on an existing track, or must stand alone.

---

## Phase 4 — Ship

**Only on the user's explicit "go" / "ship it" / "create the repo".** This phase is
destructive and creates a remote artifact — never auto-run.

- [ ] **History check (first, before anything else).** `git log --oneline -1`. If the
      directory has git history the user intends to keep, **stop and ask** — this phase
      resets history.
- [ ] **Pre-flight** — surface identity and permissions *before* the destructive step, so
      failures land before history is reset and the commit is made:
      - **Git commit identity** — `git config user.name` / `git config user.email`; show
        the user who the first commit will be authored as and ask them to confirm or set a
        repo-local identity. In a work context, proactively suggest the work email rather
        than silently inheriting personal global config.
      - **GitHub auth** — `gh auth status`; confirm the active account carries `repo` scope.
      - **Org create-permission** (org targets only) — `gh api user/memberships/orgs/<org>`
        and inspect `role`; if not `admin`, warn that creation needs the org's
        "members can create repositories" policy, or an admin to provision.
      - **Visibility availability** — `--internal` requires GitHub Enterprise; surface that
        before the user picks visibility, not after `gh repo create` fails.
- [ ] **Pre-cleanup** — remove retrofit scaffolding whose job has ended: **this file
      (`RETROFIT.md`)**, any `CLAUDE.md.template` fetched in Phase 1, and `.gitkeep` files
      in directories that now hold real content.
- [ ] **Scrub check — before staging, on every file that will be committed:**
      - **Scaffolding gate** (run after `git add`, before `git commit`) — a file-level
        check that does not depend on which instruction the session followed:
        ```bash
        git diff --cached --name-only | grep -E '^(RETROFIT\.md|CLAUDE\.md\.template|project-bootstrap-process\.md)$'
        ```
        Any hit means pre-cleanup did not run — **stop and remove them.**
      - **Origin traces** — `grep -riE 'retrofit|bootstrap|template' CLAUDE.md docs/ prompts/ README.md 2>/dev/null`.
        Populated files must carry no trace of how the repo came to exist. Expect
        legitimate false positives on projects that genuinely use templates (email
        templates, report templates) — judge each hit, do not blanket-scrub.
      - **Sensitive specifics that survived Phase 1 routing** — employer and internal
        product names, real end-user names and locations, cluster/host names, credentials
        and token workflows, customer or demo dates. Anything found moves to
        `CLAUDE.local.md`; the committed file refers to it generically. **If the repo will
        be public, surface a ship-as-is / redact / stay-private decision to the user — do
        not decide this silently.**
- [ ] **Write `README.md`** — brief: one short paragraph on what the project is and who
      it is for, then a pointers section linking to `docs/requirements.md`, `CLAUDE.md`,
      and anything else relevant. Detail lives in the linked files. Do **not** mention the
      retrofit, the template, or how the repo originated.
- [ ] **Reset history and stage** — `rm -rf .git && git init && git add .` (skip the `rm`
      if there is no `.git`). Run `git status`, then draft a first-commit message
      describing *the project* — e.g. "Initial commit: <one-line project description>".
- [ ] **Stop. Show the staged file tree and the proposed commit message in full. Wait for
      green-light.** Show the message **in full**, not summarized — this is the first time
      the project is stated back concisely enough to react to, and it reliably draws out
      scope corrections that open-ended questions earlier did not. It is a last-chance
      scope check, not just message approval. Do not streamline it away.
- [ ] **On confirmation** — `git commit -m "<approved message>"`, then
      `gh repo create <project-name> <--private|--public> --source . --push`. **Confirm
      visibility with the user before issuing the command** — there is no default.
- [ ] **Report and harvest** — give the user the repo URL and confirm the first commit is
      clean. Then prompt: *"Did anything in this retrofit process create friction? This is
      the moment to capture improvements while context is fresh."* Capture in
      `collateral/bootstrap-friction.md` (gitignored), one entry per item as
      **What hit** / **Why it's a problem** / **Suggested fix**. Then recommend they exit
      and start a fresh session for project work — `CLAUDE.md` is now the source of truth.

---

## Principles for Retrofit

- **The artifacts are half the spec.** Code shows what was built. Only the user knows what
  was intended, what was abandoned, and what is load-bearing. Interview before writing.
- **Phase 0 is not optional and not reorderable.** The whole reason a retrofit is needed is
  that the project cannot currently be exited and resumed. Fix that before anything else.
- **Distill, don't transcribe.** `CLAUDE.md` and the system prompt are the sharpened
  extract of the project, not an inventory of its files.
- **Don't invent scope, and don't invent tracks.** "None yet" is a real answer. Record the
  structure, not imagined content.
- **Align scaffolding, not substance.** Working code stays where it is. Every move of an
  existing file is a proposal.
- **Sensitive specifics are routed at write time,** not caught at the push gate. Catching
  them late is rework; missing them is a leak.
- **Retrofit scaffolding is template-repo property. None of it ships downstream.** This
  file and `CLAUDE.md.template` are deleted in Phase 4 pre-cleanup. If any instruction
  anywhere appears to say otherwise, this rule wins — do not adjudicate it mid-ship.
- **GitHub artifacts are project-facing.** The README, commit messages, and PR
  descriptions describe *the project*, never the retrofit or the template it borrowed
  from. The repo stands on its own.
