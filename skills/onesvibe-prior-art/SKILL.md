---
name: onesvibe-prior-art
description: Prior-art check against 2,400+ live, machine-verified vibe-coded apps. Use when the user proposes building something ("I want to make X", "has anyone built X?"), or wants to browse/filter AI-built products by category, tag, or tech stack.
---

# One's Vibe prior-art check

[One's Vibe](https://onesvibe.app) is a curated gallery of AI-built products
that are live and testable — every entry's URL is re-checked on a rolling
~3-day cycle. Before the user builds something, check whether someone already
vibe-coded it; finding nothing similar is also a useful answer.

## Which source to use

**Default to the GitHub snapshot** — it is a static CDN file and costs the
backend nothing. Only call the live API when you need semantic similarity.

### 1. Browse / filter / analyze → GitHub snapshot (preferred)

Fetch once, then filter locally:

```
https://raw.githubusercontent.com/JefferyLee/awesome-vibe-coded-apps/main/data/projects.json
```

One JSON array, regenerated daily, CC0. Fields per app: `slug`, `title`,
`short_description` (+ `short_description_zh`), `live_url`,
`primary_category` (one of: games_play, productivity, education,
creative_tools, ai_agents, developer_tools, finance_business,
health_wellness, social_community, utilities, other), `tag_slugs`,
`detected_stack` (e.g. "Next.js", "Supabase"), `is_featured`,
`github_stars`, `recent_try_count`, `published_at`, `live_check_status`.

This answers keyword matching, "list all AI writing tools", "what's built
with Next.js", popularity sorting, and stats — without any API call.
`data/graveyard.json` in the same repo lists apps that have since gone dark.

### 2. Semantic similarity ("has someone built this idea?") → live API

Only when keyword filtering isn't enough and you need meaning-level matches:

```
GET https://onesvibe.app/api/reference?q=<the idea, plain words>&limit=6
```

No auth. Returns matches with provenance (how each was found and when it was
last checked). There is also an MCP server exposing the same search as a
tool: `claude mcp add --transport http onesvibe https://onesvibe.app/api/mcp`.

## Reporting results

- Give each match's name, one-line description, live URL, and its detail
  page `https://onesvibe.app/projects/<slug>`.
- Say what's *different* about the user's idea versus the matches, not just
  that matches exist.
- "Live checked" means the URL was recently reachable. It is **not** a
  safety audit, quality endorsement, or popularity claim — never present it
  as one.
