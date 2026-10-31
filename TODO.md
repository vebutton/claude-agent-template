# [Project Name] — ToDo

> The single durable task register. Committed, so every line stays generic: people's
> names, hosts, internal product names, credentials and customer-tied dates live in
> `CLAUDE.local.md` and are referenced, never copied here. Build and resolution dates
> are fine.

## Rules
- **ID:** `T-<SLUG>-<nn>`. Permanent, never reused, never renumbered. Numbered per track.
- **Slugs** come from CLAUDE.md → Tracks, plus `PROJ` for project/repo plumbing that
  belongs to no track. A new track is added to CLAUDE.md first, by agreement, never
  invented here.
- **An item is work that finishes.** Anything that recurs, or would never close, is a
  Standing operation (section 4): no ID, no checkbox.
- **Track and deliverable are fixed at creation.** If the deliverable changes, close the
  item with a pointer and open a new one that references the old ID. Don't retitle.
- **New items** append at the end of their track. Propose each one with a proposed ID
  and wait for agreement.
- **Closed items** stay in place, ticked: `(resolved YYYY-MM-DD: outcome)`. Move them to
  Archive only when the resolution no longer informs current work.
- **Item detail is current state only:** what's left, what it waits on, priority,
  dependencies, pointers. What happened and when goes in `docs/session-state.md`.
- **Open questions live only here** (section 3). Requirements and design docs point to
  this section instead of keeping their own lists.
- **This is the only durable task list.** The in-session todo tool is scratch. At end of
  session, reconcile it into this file, and give every open or next-step line written to
  `docs/session-state.md` an ID or an Unscheduled line, or drop it deliberately.
- **Refer to items by ID everywhere** (CLAUDE.md, docs, commit messages), never by
  position ("pickup item 3"). A commit that does an item's work names its ID.

## 1. Items

### PROJ — Project and repo plumbing
- [ ] T-PROJ-01 [First plumbing item, e.g. "Create the remote repo"]

### [SLUG] — Track 1: [Name]
- [ ] T-[SLUG]-01 [First concrete step toward Track 1's objective]

## 2. Item detail
Only for items that need more than one line. Grouped by track, in section 1 order.

## 3. Open questions
Each one names the item that will resolve it, or is marked accepted risk.

## 4. Standing operations
Recurring work: no IDs, no checkboxes, tracked by last-run date.

| Operation | Cadence / trigger | Last run | Notes |
|---|---|---|---|

## 5. Unscheduled
Ideas that came up but aren't committed work. No ID until promoted into a track.

## 6. Archive
Closed items whose resolution no longer informs current work.
