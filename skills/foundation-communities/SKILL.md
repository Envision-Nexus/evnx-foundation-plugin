---
name: foundation-communities
description: Use when looking up EVNX Foundation communities, their members or the people directory through the evnx-foundation MCP tools (list_my_communities, list_community_members, search_people) - e.g. "which communities am I in", "who's in community X", "find people who know Y".
---

# EVNX Foundation: communities and people

Tools: `list_my_communities`, `list_community_members`, `search_people`. All read-only in v1.

- `list_my_communities()` takes no arguments — it is always scoped to the token's own owner, never another person.
- `list_community_members({ slug })` needs the community's own slug, not a Space's. Use `list_my_communities` first if you don't have it.
- `search_people({ query?, limit? })` searches the whole platform directory (not scoped to one community); `limit` caps at 50, default 24.

## Common mistakes

- Community slugs and Space slugs are different values that can look similar — don't assume a Space slug works on `/api/v1/communities/...` routes.
- `list_community_members` 404s for communities the token's owner doesn't belong to, by design (no outsider preview via the API).
