---
name: foundation-my-tasks-triage
description: Use when the person asks to triage, review or get an overview of their open tasks across one or more EVNX Foundation Spaces - e.g. "what should I focus on today", "triage my tasks in Space X".
---

# Workflow: My Tasks triage

A read-mostly workflow built on `foundation-tasks`'s tools. Goal: turn a pile of open tasks into a short, prioritized list the person can act on. This workflow does not bulk-edit anything — it only reads and suggests; any change (moving a task, changing priority) is a separate, explicit `update_task` call the person confirms first.

## Steps

1. Ask which Space(s) to triage if not given (or use `list_my_communities` + ask which Spaces, since there's no "my Spaces" tool yet — the person usually just names the Space).
2. For each Space: `list_space_tasks({ slug, status: "todo" })` and `list_space_tasks({ slug, status: "in_progress" })` (skip `backlog`/`review`/`done` unless asked).
3. Sort what you found: overdue (`due_date` in the past) first, then `urgent`/`high` priority, then everything else, then no-due-date tasks last.
4. Flag anything in `depends_on` that is still open elsewhere — call out tasks that look stuck.
5. Present a short list (title, priority, due date, who it's assigned to) grouped as: **Overdue**, **Due soon**, **In progress**, **Everything else** (collapse this last group if it's long — say how many, not every title).
6. Ask before taking any action (e.g. "want me to bump the due date on X?") — never call `update_task` as part of the triage itself.

## When this doesn't fit

If the person already named a specific task to change ("mark X done"), skip the workflow and just call `update_task` directly — triage is for the open-ended "what should I look at" case.
