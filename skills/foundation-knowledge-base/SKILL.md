---
name: foundation-knowledge-base
description: Use when reading or writing EVNX Foundation community knowledge base pages through the evnx-foundation MCP tools (list_knowledge_pages, get_knowledge_page, create_knowledge_page, update_knowledge_page_text) - e.g. "what does our KB say about X", "write up a KB page for Y".
---

# EVNX Foundation: knowledge base

Tools: `list_knowledge_pages`, `get_knowledge_page`, `create_knowledge_page`, `update_knowledge_page_text`.

## Scope (read this before writing anything)

The knowledge base in the app is a full block editor (headings, tables, embeds, pictures, checklists). These MCP tools are a **plain-text-only** slice of it:

- `get_knowledge_page` returns `doc` (the raw block JSON — don't try to parse or render it yourself) and `text` (the page's plain text — use this one).
- `update_knowledge_page_text` and `create_knowledge_page` only ever write a SINGLE plain-text paragraph as the body. Calling `update_knowledge_page_text` on a page that has real formatting (tables, headings, pictures) REPLACES all of that with plain text. Always warn the person before overwriting a page that has more than plain text, and prefer appending a new page over rewriting an existing richly-formatted one.
- There is no delete, move, or per-page-permission tool here — those stay in the web app.

## Finding pages

`list_knowledge_pages({ slug })` (community slug, not a Space) returns the whole tree as a flat list of `{ id, parent_id, title, position }` — build the hierarchy yourself from `parent_id` if you need to show structure. It does not include drafts/archived pages' content, just their presence.

## Creating a page

`create_knowledge_page({ slug, title })` makes a TOP-LEVEL page only (no nesting via this tool). If the person wants it nested under another page, create it, then tell them to drag it into place in the app (or just do the flat create and say so).

## Common mistakes

- Don't treat `doc` as something you can edit and send back — only `text` round-trips meaningfully through these tools.
- A community's knowledge base needs membership to read or write (editors: Unlockers always, others only if ticked) — a 404 here usually means the signed-in person (the token owner) isn't a member or editor, not that the page doesn't exist.
