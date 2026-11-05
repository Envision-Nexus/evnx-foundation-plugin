---
name: foundation-tasks
description: Use when working with EVNX Foundation tasks, comments, milestones, sprints or approvals through the evnx-foundation MCP tools (list_space_tasks, get_task, create_task, update_task, list_task_comments, add_task_comment, list_space_milestones, create_space_milestone, update_space_milestone, list_space_approvals, list_my_approvals, list_space_sprints, create_space_sprint, update_space_sprint) - e.g. "what's open in Space X", "create a task", "move this to done", "add a comment", "plan a sprint", "what's waiting on my approval".
---

# EVNX Foundation: tasks, milestones, sprints and approvals

Tools: `list_space_tasks`, `get_task`, `create_task`, `update_task`, `list_task_comments`, `add_task_comment`, `list_space_milestones`, `create_space_milestone`, `update_space_milestone`, `list_space_approvals`, `list_my_approvals`, `list_space_sprints`, `create_space_sprint`, `update_space_sprint`.

## Identifying a Space

Every task/milestone/sprint tool takes `slug`, the Space's URL slug (the part after `/s/` in `https://foundation.evnx.community/s/<slug>`), not its display name. If you only have a name, ask the person for the slug or the Space's URL — there is no "search Spaces by name" tool yet.

## Reading tasks

- `list_space_tasks({ slug })` returns every task; pass `status` (`backlog` | `todo` | `in_progress` | `review` | `done`) to filter.
- Each task has `assignees` (array of `{ user: { id, username, full_name } }`), `labels`, `priority`, `due_date`, `depends_on` (task ids this one is blocked by).
- `get_task({ slug, taskId })` for one task's full detail.

## Creating a task

`create_task({ slug, title, status? })` adds a task at the END of that column (default `todo`). This mirrors the "+ Add a task" quick-add row in the app — it does NOT set assignees, labels, due date or a milestone at creation. Set those afterwards with `update_task`.

## Updating a task

`update_task({ slug, taskId, title?, description?, priority?, due_date?, status?, assignees? })`. Omit a field to leave it unchanged.
- `title`: plain text, up to 200 characters.
- `description`: plain text, up to 10,000 characters — this **REPLACES** any existing description, including pictures or rich formatting it had. Warn the person before overwriting a description that has more than plain text.
- `priority`: `low` | `medium` | `high` | `urgent`.
- `due_date`: an ISO date string, or `null` to clear it.
- `status`: moving a task is refused (400, "blocked by an unfinished dependency") if it still depends on an unfinished task — check `depends_on` first if a move fails.
- `assignees`: a list of usernames or user ids, matched against this Space's members; anyone not a member is silently dropped.

## Comments

`list_task_comments({ slug, taskId })` returns comments oldest first. `add_task_comment({ slug, taskId, text })` posts a plain-text comment (up to 5,000 characters); `@username` notifies that Space member.

## Milestones

`list_space_milestones({ slug })` lists them. `create_space_milestone({ slug, title, description?, due_date?, status? })` and `update_space_milestone({ slug, milestoneId, title?, description?, due_date?, status? })` create/edit one (`status`: `planned` | `in_progress` | `done` | `missed`). If the community requires approval before a milestone counts "done", setting `status: "done"` does **not** mark it done — it starts an approval request instead, same as the web app. Check the response's `approvalRequested` flag before telling the person it's done.

## Approvals

`list_space_approvals({ slug })` lists every approval request in a Space (needs `budget.view`, same gate as the web app's Approvals tab). `list_my_approvals({ role? })` lists the signed-in person's own — `role: "requested"` (default) for ones they asked for, `"waiting"` for ones waiting on them to decide. There is no "decide an approval" tool yet — point the person to the app for that.

## Sprints

`create_space_sprint({ slug, name, goal, startDate, endDate })` and `update_space_sprint({ slug, sprintId, name, goal, startDate, endDate })` plan or edit a sprint (dates are ISO, `endDate` must not be before `startDate`). This is the same form the web app's "New sprint" / "Edit sprint" uses — it does NOT start, close or assign tasks to a sprint; that stays web-app-only.

## Files on tasks

To read a picture, PDF or video already attached to a task's description or a comment, or to attach a new one, see the `get_file` / `upload_file` tools (shared with the knowledge base — not specific to this skill).

## Not supported via the API

Labels, subtasks, milestone links beyond status/title/description/due date, starting/closing a sprint or moving tasks into one, and deciding an approval. Say so plainly rather than guessing a workaround; point the person to the task's, milestone's or sprint's page in the app.

## Common mistakes

- Don't invent a Space slug from a guessed name — ask, or use the URL the person gave you.
- A 404 from any of these tools means "doesn't exist, or you can't see it" by design (Foundation never reveals which) — don't retry with a different casing or similar slug hoping for a match.
- `list_space_tasks` can return a lot of tasks for a busy Space; filter by `status` before dumping the whole list into a long answer.
- Don't assume a milestone was marked done after `update_space_milestone` — check `approvalRequested` first.
