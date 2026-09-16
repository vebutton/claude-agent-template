---
description: Retrofit an in-flight, never-bootstrapped project onto the template's footing (pulls the canonical checklist fresh from claude-agent-template on GitHub)
allowed-tools: Bash(gh api:*)
---

The canonical retrofit checklist follows, pulled live from the
`vebutton/claude-agent-template` repo. **Honour every instruction in it** —
especially the "When this applies" preconditions, the Phase 0 ordering rule,
and the Phase 4 scaffolding gate.

Operating notes for THIS invocation:

1. **Check the preconditions before starting.** If the directory has git
   history the user intends to keep, or already has a populated `CLAUDE.md`
   and `docs/requirements.md`, stop and say so — this is not a retrofit
   candidate.
2. **Phase 0 first, then stop and report.** Do not run straight through all
   four phases. Phase 0 exists so the user can exit cleanly; confirm it landed
   and let them decide whether to continue in this session.
3. **The checklist is inlined below, not written to disk.** There is no
   `RETROFIT.md` file in the project to clean up in Phase 4 — but if you
   fetched `CLAUDE.md.template` to a file in Phase 1, that one still must be
   deleted at pre-cleanup.
4. **Phase 4 needs an explicit "go" from the user.** Never auto-run it.

---

!`gh api -H "Accept: application/vnd.github.raw" repos/vebutton/claude-agent-template/contents/RETROFIT.md`
