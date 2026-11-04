---
name: foundation-space-weekly-digest
description: Use when the person wants a weekly summary or digest of one EVNX Foundation Space's activity - e.g. "summarize this week in Space X", "what happened in Space X this week".
---

# Workflow: Space weekly digest

Read-only workflow. Produces a short written summary of a Space's week from what the API can see — it is NOT the same as the app's own "Recent activity" feed (which has more detail, e.g. flags and retro notes, not exposed via the API yet). Say so if asked for something this can't show.

## Steps

1. Get the Space slug (ask if not given).
2. `list_space_tasks({ slug, status: "done" })` — filter client-side to ones updated in the last 7 days (compare `updated_at`) for "completed this week".
3. `list_space_tasks({ slug })` (no filter) — filter to tasks created in the last 7 days (`created_at`) for "new this week".
4. `list_space_milestones({ slug })` — note any with a `due_date` in the next 7 days, or whose `status` changed to `done` recently (best-effort: milestones don't carry an `updated_at` here).
5. `list_space_sprints({ slug })` — mention the active sprint, if any, and roughly how it's tracking (done vs total tasks in it, from the task list's `sprint_id`).
6. Write a short digest: **Completed this week** (titles, who did them if obvious from `assignees`), **New this week**, **Milestones / sprint**, and one line on anything that looks overdue or stuck (same signal as the triage workflow: `due_date` in the past, or `depends_on` pointing at open tasks).

Keep it to a few short paragraphs or a compact bullet list — this is a digest, not a full report.
