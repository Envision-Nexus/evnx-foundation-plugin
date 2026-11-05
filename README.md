# EVNX Foundation plugin for Claude

Lets Claude read and make changes to your [EVNX Foundation](https://foundation.evnx.community) communities and Spaces — tasks, milestones, sprints, approvals, knowledge base pages, members and people — through your own personal API token.

## Install

1. In Foundation, go to **Settings → API tokens**, create one, and copy it (it's shown once).
2. Install the plugin (see the marketplace entry in `../marketplace.json`, or add this plugin's directory directly with `claude plugin install`/`/plugin install`). Claude Code will prompt for the token during install and store it securely — it isn't kept as a plain environment variable.

The plugin's MCP server is a small proxy hosted by EVNX (`mcp.foundation.evnx.community`) — it never stores your token, it only forwards it to Foundation's own API on each call. The token acts as you everywhere in Foundation your account can already reach; revoke it any time from Settings → API tokens.

## What it can do

- **Tasks**: list, read, create, and update tasks (title, description, priority, due date, assignees, status/column); read and post comments.
- **Milestones & sprints**: list, create, and update milestones (an approval may be required before one counts "done", same as the web app) and sprints.
- **Approvals**: read a Space's approval requests, or the signed-in person's own (requested, or waiting on them to decide).
- **Files**: read a picture, PDF or video attached to a task, comment or knowledge base page; upload one to attach to a Space's task or comment.
- **Knowledge base**: list a community's page tree, read a page, create a top-level page, replace a page's plain-text body.
- **Communities & people**: list your communities, a community's members, and search the people directory.

See `skills/` for the exact tool contracts and gotchas, and the two workflow skills (`foundation-my-tasks-triage`, `foundation-space-weekly-digest`) for ready-made "triage my tasks" / "summarize this week" flows.

## What it can't do (yet)

Rich knowledge base formatting (tables, headings, embeds, drawings — only plain text round-trips through these tools), uploading a file straight into a knowledge base page (only Space task/comment attachments), subtasks, labels, starting or closing a sprint, budgets/money requests, points, meetings, and the AI companion are web-app-only for now — this is a v1 slice of the platform, not the whole thing.
