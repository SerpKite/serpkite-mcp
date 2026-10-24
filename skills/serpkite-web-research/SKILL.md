---
name: serpkite-web-research
description: Search Google, read public webpages, get cited answers and run deep research through the SerpKite MCP server. Use when a task needs current web results, news, maps, scholar or patent results, shopping results, the Markdown text or structured JSON of a public page, a cited answer, or a researched report.
---

# SerpKite web research

## Setup

The `serpkite` MCP server reads the API key from the `SERPKITE_API_KEY` environment variable and sends it as `Authorization: Bearer <key>`. Get a key at https://app.serpkite.com. Never paste the key into chat, commit it, or print it in logs.

If the server reports 401, the variable is missing or the key is wrong. Ask the user to set `SERPKITE_API_KEY` and restart Cursor.

## Tools

- `search`: Google web results. Start here for general questions.
- `news`: recent articles. Use for anything time-sensitive.
- `images`, `videos`, `shopping`, `maps`: vertical results.
- `place`: one place's full details, using the `place_id`, `cid` or `fid` from a `maps` result.
- `scholar`, `patents`: academic papers and patents.
- `autocomplete`: query suggestions.
- `webpage`: one public URL as Markdown. Use it to read a result in full.
- `answer`: a short answer to a question from live Google results, with numbered citations. `depth=fast` uses only the result page; `standard` also reads the top pages.
- `extract`, `map`, `crawl`, `crawl_result`: multi-page site reads. `extract` can also return query passages, a summary or JSON matching an `output_schema`. `crawl` is asynchronous; poll `crawl_result`.
- `research`, `research_result`: asynchronous deep research (planned searches, page reading, a cited Markdown report). Start with `research`, then poll `research_result`. Use it only when a multi-source report is worth the higher cost.

## Guidelines

1. Each tool call spends the user's SerpKite credits. Failed and empty searches are not billed. Prefer one precise query over many broad ones.
2. Read the top results' snippets first, then fetch only the pages you need with `webpage`.
3. Cite the URLs you used.
4. Only public, logged-out pages are available. Don't try to read content behind a login.
