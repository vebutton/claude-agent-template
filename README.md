# claude-agent-template

A scaffold for kicking off new agent and app projects with Claude Code. You have a
conversation with Claude about what you want to build, drop that conversation into a
clone of this template, and Claude Code bootstraps the repo from it — project context,
behavior rules, requirements doc, tooling — before writing any real code.

## Starting a new project

**Clone the template locally. Do not create the remote repo yet.**

```bash
git clone https://github.com/vebutton/claude-agent-template.git my-new-project
cd my-new-project
rm -rf .git   # optional — bootstrap step 13 does this anyway
```

Then drop the defining conversation into `collateral/` and open Claude Code in the
directory. It reads `CLAUDE.md` automatically and works through the bootstrap checklist.

**Do not use GitHub's "Use this template" button, and do not run `gh repo create` up
front.** Both create a remote immediately, which forces the owner/org and visibility
decision at the moment you have the least context — and GitHub defaults to your personal
account. The bootstrap process resets git history and creates the remote at ship time,
with pre-flight checks for commit identity, `gh` auth scope, org create-permission, and
visibility. Any remote created before that point is throwaway.

## What you get

| Path | Role |
| --- | --- |
| `CLAUDE.md` | Bootstrap checklist on first run; becomes the project's persistent context after |
| `CLAUDE.md.template` | Target structure the populated `CLAUDE.md` is written into |
| `prompts/system_prompt.md` | Agent behavior rules — tone, refusals, output discipline (agent projects only) |
| `docs/session-state.md` | Backward-looking session archaeology; the forward-looking brief lives in `CLAUDE.md` |
| `collateral/`, `output/`, `src/`, `docs/` | Working directories, gitignored where appropriate |

No `pyproject.toml`, `.python-version`, or `requirements.txt` ships here — each project
picks its own stack during bootstrap, or explicitly defers the decision.

## Pointers

- [project-bootstrap-process.md](project-bootstrap-process.md) — the full workflow:
  having the conversation, capturing it cleanly, extracting text from decks and email
  threads, and shipping the finished repo.
- `CLAUDE.md` — the bootstrap checklist itself, and the principles behind it.
