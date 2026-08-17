# Project Bootstrap Workflow

A process for kicking off new agent or app projects from a conversation — voice
(Claude iOS app), text (claude.ai browser), or a mix of the two. The conversation
captures the substance; this workflow turns it into a working repo.

## Steps

1. **Have the conversation (any Claude interface)**
   - **⚠️ Do NOT use incognito mode.** Claude on iOS has an incognito mode that does not
     save the conversation. The entire bootstrap pattern depends on the defining
     conversation being retrievable as text afterwards — incognito silently breaks the
     pipeline at its source, and you only find out once the conversation is over and
     unrecoverable. (Learned the hard way: one project's conversation had to be
     reconstructed from ~32 screenshots.)
   - **Voice (iOS app):** great for brainstorming while driving, walking, or at lunch.
     Claude asks clarifying questions and helps refine the concept.
   - **Text (claude.ai browser):** great when you already have artifacts (screenshots,
     existing files, URLs) to paste in, or when you want more structured back-and-forth.
   - **Mix:** start voice on the go, continue in the browser when you're at a computer —
     conversations sync across both.

2. **Copy the full conversation**
   - Open [claude.ai](https://claude.ai) in a browser on your computer.
   - Navigate to the conversation (it syncs from iOS if you started there).
   - Select the thread and copy it as **plain text**.
   - **Avoid the "Copy as GFM" export button.** Despite the label and the `.md` extension
     you save it under, it produces HTML — inline `style="..."` on every `<div>`, with the
     conversation text buried inside. A 450 KB export is ~95% markup, and reading it in
     Claude Code burns token budget for almost no signal. If you end up with one anyway,
     see the cheatsheet below.
   - **Check the export isn't truncated.** Scroll to the end of the pasted file and confirm
     it stops at a clean handoff, not mid-thread. Truncated exports lose exactly the
     decisions made last — often including the project name.

3. **Paste into a `.md` file**
   - Open an editor (VSCode, Atom, emacs, etc.).
   - Create a new empty `.md` file (e.g., `my-agent-conversation.md`).
   - Paste the full conversation.
   - Pasting into Word first gives cleaner visual formatting, but `.md` is preferred for portability.
   - This file is a working input — it does NOT stay in the repo long-term (gitignored, see step 8).

4. **Clone the template locally — do NOT create the remote repo yet**
   - Clone the template repo into a new directory:
     ```bash
     git clone https://github.com/vebutton/claude-agent-template.git my-new-project
     cd my-new-project
     ```
   - This gives you the full scaffold (`.gitignore`, `CLAUDE.md`, `CLAUDE.md.template`,
     `collateral/`, `output/`, `prompts/`, `src/`, `docs/`).
   - **Do not use GitHub's "Use this template" button, and do not run `gh repo create`
     now.** Both create a remote immediately, which forces the owner/org and visibility
     decision at the moment of least context — and GitHub defaults to your personal
     account. Bootstrap step 13 resets history and creates the remote fresh anyway, so
     any remote created before then is throwaway. If you already made one, treat it as
     disposable and delete or transfer it.

5. **Drop the conversation into `collateral/`**
   - Move your `.md` conversation file into the cloned repo's `collateral/` folder.
   - Drop in any other collateral the conversation refers to (templates, sample data,
     reference documents) — the conversation should name these explicitly.
   - **Multi-source is fine and common.** The collateral doesn't have to be one
     self-contained `.md`. A short pointer file ("look at the emails and the deck, ask me
     about X and Y") plus the actual `.eml` / `.pptx` / PDF sources works — non-engineer
     stakeholders send what's natural for them. Bootstrap step 2 reads everything until
     it has a coherent picture.
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

8. **Finalize and ship the repo**

   Bootstrap step 13 in `CLAUDE.md` walks Claude through this on your green-light;
   the manual flow below is the same thing. Either way, this step is destructive
   and creates a remote artifact — give Claude (or yourself) explicit go-ahead.

   Pre-flight (do this *before* `rm -rf .git`, so failures surface before history is
   reset and the commit is made, not after):
   - Confirm the git commit identity — `git config user.name` / `git config user.email`.
     In a work context, set a repo-local identity (`git config user.email <work-email>`)
     before committing rather than silently inheriting personal global config.
   - `gh auth status` — confirm the active account carries `repo` scope.
   - For an org repo: check create permission (`gh api user/memberships/orgs/<org>`,
     inspect `role`); note that `--internal` visibility requires GitHub Enterprise.

   Pre-cleanup (remove files whose job ended at bootstrap):
   - `CLAUDE.md.template` — consumed in bootstrap step 6
   - The template's `README.md` — replaced wholesale by the project README, not edited
   - `.gitkeep` files in any directory that now has real content (e.g. `src/.gitkeep`
     once source files exist)
   - For app projects, the `prompts/` directory (already removed in bootstrap step 7)

   Scrub check (before staging — the last gate before content becomes public):
   - Scaffolding gate, after `git add` and before `git commit`:
     ```bash
     git diff --cached --name-only | grep -E '^(project-bootstrap-process\.md|CLAUDE\.md\.template|README\.md)$'
     ```
     A hit on the first two means pre-cleanup didn't run — stop and remove them. A
     `README.md` hit is only correct if it's the project's own README, not the template's.
   - `grep -riE 'bootstrap|template' CLAUDE.md docs/ prompts/` — populated files should
     carry no bootstrap or template references. Session State and Project Status written
     during bootstrap are the usual offenders. Legitimate uses of the word "template"
     (email templates, report templates) are false positives — judge, don't blanket-scrub.
   - Sensitive specifics: employer and internal product names, real end-user names and
     locations, cluster/host names, credentials, customer dates. These belong in
     `CLAUDE.local.md` (gitignored); committed files refer to them generically. For a
     public repo, decide consciously — ship as-is, redact, or stay private.

   Reset git history so the template scaffolding doesn't carry into the project:
   ```bash
   rm -rf .git
   git init
   git add .
   git status   # review the staged tree before committing
   ```

   Review the staged tree and draft the first commit message, then:
   ```bash
   git commit -m "<first commit message>"
   gh repo create my-new-project <--private|--public> --source . --push
   ```

   What to commit:
   - `CLAUDE.md` — populated project context, loaded by Claude Code every session
   - `prompts/system_prompt.md` — agent behavior rules (agent projects only)
   - `docs/` — structured requirements and specs
   - `pyproject.toml`, `.python-version`, `uv.lock` (Python projects)
   - Source code as it develops

   What NOT to commit — bootstrap scaffolding (removed in pre-cleanup, NOT gitignored,
   so nothing catches these automatically):
   - `project-bootstrap-process.md` — this doc. It is template-repo property and does
     not ship downstream. A downstream repo carrying it announces that it came from a
     template, which the template's own principles forbid.
   - `CLAUDE.md.template` — consumed at bootstrap step 6
   - The template's `README.md` — replaced by the project's own

   What NOT to commit — already gitignored:
   - The raw conversation `.md` file in `collateral/` — one-time input
   - `TODO.md` — local backlog, not part of the active repo
   - `.env` / secrets

   If anything in the template made this bootstrap harder than it should have been,
   capture it in `collateral/bootstrap-friction.md` (gitignored — stays local) using
   **What hit / Why it's a problem / Suggested fix**, for later harvest back into the
   template repo.

   After the repo is created, exit this session and start a fresh one in the same
   directory for project work — the populated `CLAUDE.md` is now the source of truth.

## Extracting Text From Common Collateral Formats

Stakeholders send what's natural for them — decks, email threads, browser exports — not
clean markdown. These are the paths that work, so a bootstrap session doesn't have to
rediscover them:

- **HTML-laden `.md`** (claude.ai "Copy as GFM" export): the conversation text sits inside
  `<div>`s carrying huge inline `style="..."` attributes. Don't `Read` it directly — the
  first chunk alone can exceed the tool's token limit. Strip tags first (a few lines of
  `HTMLParser`, or `sed`/`textutil`), keeping the user/assistant turn boundaries. Better
  still, avoid producing one: copy as plain text (step 2).
- **`.pptx` slide text:**
  ```bash
  unzip -o deck.pptx 'ppt/slides/*.xml' -d /tmp/deck
  grep -oh '<a:t>[^<]*' /tmp/deck/ppt/slides/*.xml | sed 's/<a:t>//'
  ```
- **`.pptx` embedded images:** `unzip -o deck.pptx 'ppt/media/*' -d /tmp/deck`, then `Read`
  each PNG/JPG — screenshots and diagrams often carry the substance the text doesn't.
- **`.eml` with a large attachment:** `Read` works on small ones, but a multi-megabyte
  base64 attachment swamps the human-readable text. Pull just the headers and the
  `text/plain` part:
  ```bash
  python3 -c "import email,sys; m=email.message_from_file(open(sys.argv[1])); \
    print(m['from'], m['date'], m['subject']); \
    print(next(p.get_payload(decode=True).decode('utf8','replace') \
      for p in m.walk() if p.get_content_type()=='text/plain'))" thread.eml
  ```

## Why CLAUDE.md Is the Key File

Claude Code automatically reads `CLAUDE.md` at the start of every session — it is
the project's persistent context. Unlike the conversation file (a one-time input),
`CLAUDE.md` travels with the repo and keeps any Claude Code session instantly oriented.
Think of it as the README *for Claude*, not for humans.

For agent projects, `prompts/system_prompt.md` is the companion file — it holds
behavior rules (tone, what the agent refuses, output discipline) while CLAUDE.md
holds project context. Don't duplicate between them.

## Why a GitHub Repo

- Requirements docs and code should be version-controlled from day one.
- GitHub gives you backup, history, and the ability to work across machines.
- It keeps each project separate and namespaced correctly.
- Creating the repo after bootstrap means the first commit is the real project,
  not the template scaffolding — clean history from the start.

Visibility (private vs. public) is a per-project decision — roughly half the
projects in this workflow are public, half private. Choose at repo-create time.

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
