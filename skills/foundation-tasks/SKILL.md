---
name: foundation-tasks
description: Use when working with EVNX Foundation tasks, milestones or sprints through the evnx-foundation MCP tools (list_space_tasks, get_task, create_task, update_task, list_space_milestones, list_space_sprints, create_space_sprint, update_space_sprint) - e.g. "what's open in Space X", "create a task", "move this to done", "plan a sprint".
---

# EVNX Foundation: tasks

Tools: `list_space_tasks`, `get_task`, `create_task`, `update_task`, `list_space_milestones`, `list_space_sprints`, `create_space_sprint`, `update_space_sprint`.

## Identifying a Space

Every task tool takes `slug`, the Space's URL slug (the part after `/s/` in `https://foundation.evnx.community/s/<slug>`), not its display name. If you only have a name, ask the person for the slug or the Space's URL — there is no "search Spaces by name" tool yet.

## Reading tasks

- `list_space_tasks({ slug })` returns every task; pass `status` (`backlog` | `todo` | `in_progress` | `review` | `done`) to filter.
- Each task has `assignees` (array of `{ user: { id, username, full_name } }`), `labels`, `priority`, `due_date`, `depends_on` (task ids this one is blocked by).
- `get_task({ slug, taskId })` for one task's full detail.

## Creating a task

`create_task({ slug, title, status? })` adds a task at the END of that column (default `todo`). This mirrors the "+ Add a task" quick-add row in the app — it does NOT set assignees, labels, due date or a milestone. To set those, create the task, then `update_task` for the fields it supports (see below), or tell the person the rest needs the web app for now.

## Updating a task

`update_task({ slug, taskId, priority?, due_date?, status? })` is deliberately narrow:
- `priority`: `low` | `medium` | `high` | `urgent`.
- `due_date`: an ISO date string, or `null` to clear it.
- `status`: moving a task is refused (400, "blocked by an unfinished dependency") if it still depends on an unfinished task — check `depends_on` first if a move fails.

## Sprints

`create_space_sprint({ slug, name, goal, startDate, endDate })` and `update_space_sprint({ slug, sprintId, name, goal, startDate, endDate })` plan or edit a sprint (dates are ISO, `endDate` must not be before `startDate`). This is the same form the web app's "New sprint" / "Edit sprint" uses — it does NOT start, close or assign tasks to a sprint; that stays web-app-only.

## Not supported via the API

Editing a task's title/description, assignees, labels, milestone, subtasks; starting/closing a sprint or moving tasks into one. Say so plainly rather than guessing a workaround; point the person to the task's or sprint's page in the app.

## Common mistakes

- Don't invent a Space slug from a guessed name — ask, or use the URL the person gave you.
- A 404 from any of these tools means "doesn't exist, or you can't see it" by design (Foundation never reveals which) — don't retry with a different casing or similar slug hoping for a match.
- `list_space_tasks` can return a lot of tasks for a busy Space; filter by `status` before dumping the whole list into a long answer.
