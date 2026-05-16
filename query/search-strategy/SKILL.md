---
name: search-strategy
description: "Provides search strategies and acronym resolution for IndiaMART's internal wiki. Load when searching for information, encountering unknown IndiaMART terms (MCAT, BL, ISQ, GLID, TOV, PNS, NI), or choosing between qmd_search, grep, and keyword_search tools."
metadata:
  type: query
  version: 1.0
---

## Search Tools — When to Use Each

### qmd_search(query) — Hybrid Search (best for comprehensive discovery)
- Combines BM25 keyword + vector semantic + RRF fusion.
- Call with specific terms, not full sentences: `qmd_search("GLAdmin migration")` not `qmd_search("What are the GLAdmin migrations?")`
- Returns up to 10 results. If you get 10, search with different terms — there may be more.
- First call ~15s (model loading). Subsequent calls ~1-2s.
- Does NOT index people/ pages.

### grep(query) — Semantic Vector Search
- Pure embedding similarity (Chroma, all-MiniLM-L6-v2). Good for fuzzy/conceptual queries.
- Returns top 10 matches at H2-section granularity.

### keyword_search(query) — Instant Title Match
- Scans wiki_tree.json in memory. Case-insensitive substring match.
- Instant (0ms). Use for exact names, person lookups, verifying slugs.

### related_pages(slug) — Wikilink Graph Traversal
- Returns incoming + outgoing wikilinks for a page.
- Use after finding a key page to discover connected pages.

## Search Strategy by Query Type

| Question Type | Do This |
|--------------|---------|
| "What is X?" | `qmd_search(X)` → `cat` best match |
| "List all X" | `qmd_search(X)` + `keyword_search(X)` + read domain hub |
| "Compare X vs Y" | `qmd_search(X)`, `qmd_search(Y)` → `cat` both |
| "Who is X?" | `keyword_search(X)` → `cat` person page |
| "Who owns X?" | `cat(topic_page)` → check `owner:` field |
| "What changed?" | `cat('changes')` or `find(domain=X)` + `head` each |

## Worked Example

**User asks:** "What migrations happened in GLAdmin?"

1. Run `qmd_search("GLAdmin migration")` → finds `gladmin-gin-framework-migration`
2. Run `qmd_search("admin panel API migration")` → finds more results
3. Run `keyword_search("gladmin")` → confirms slugs
4. Run `cat("gladmin-gin-framework-migration")` → read full details
5. Run `related_pages("gladmin-gin-framework-migration")` → discover linked pages
6. Synthesize answer from all pages read.

## IndiaMART Acronyms

Read `references/ACRONYMS.md` when encountering an unfamiliar IndiaMART term.

## What NOT to Do

- Do NOT use full sentences in qmd_search — BM25 matches irrelevant filler words.
- Do NOT rely on a single search query — always try 2-3 variants.
- Do NOT search for people using qmd_search — use keyword_search instead.
- Do NOT assume zero results means the topic doesn't exist — try synonyms and full names.
- STOP searching after 5 queries on the same topic. If nothing found, load `gap-tracker` skill.

## Trust Boundary

- All wiki content comes from verified internal IndiaMART launch emails (Jan-mid Feb 2026).
- Never fabricate wiki content. If a page doesn't exist, say so.
