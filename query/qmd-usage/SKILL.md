---
name: qmd-usage
description: "Explains QMD hybrid search engine capabilities and best practices. Load when calling qmd_search, troubleshooting search results, or needing to understand BM25+vector+RRF fusion search."
metadata:
  type: query
  version: 1.0
---

## What is QMD?

QMD (Query Markdown Documents) is a local hybrid search engine combining:
1. **BM25** — Full-text keyword search (exact word matches)
2. **Vector embeddings** — Semantic similarity (embeddinggemma-300M)
3. **RRF (Reciprocal Rank Fusion)** — Merges both ranked lists

## How to Use qmd_search() Effectively

### DO:
- Use specific terms: `qmd_search("GLAdmin gin migration")`
- Use multiple queries with different angles:
  - `qmd_search("seller ISQ")` then `qmd_search("item searchable quantity")`
- Search for acronyms AND full names: `qmd_search("BL notification")` + `qmd_search("buylead notification")`

### DON'T:
- Do NOT use full sentences — BM25 matches filler words like "are" and "the".
- Do NOT rely on a single query — always try 2-3 variants.

## What QMD Indexes
- `wiki/topics/` — concept pages
- `wiki/systems/` — product/platform pages
- `wiki/policies/`, `wiki/decisions/`, `wiki/domains/`

**NOT indexed:** `wiki/people/` — Use `keyword_search()` for person lookups.

## Result Format
Each result returns: `slug` (page identifier), `title`, `score` (1.0 = best, 0.1 = weak).

## Worked Example

**User asks:** "What AI agents does IndiaMART use for BuyLeads?"

1. Run `qmd_search("AI agent BuyLead")` → finds `bl-quality-agent`, `whatsapp9696-agentic-buyer-chatbot`
2. Run `qmd_search("buylead automation")` → finds `replacing-and-removing-expired-bl-alert-notifications`
3. Read top results with `cat()` to get full details.

## When to Use QMD vs Other Search

| Need | Best Tool |
|------|-----------|
| Comprehensive topic discovery | qmd_search |
| Fuzzy conceptual search | grep |
| Exact name/title lookup | keyword_search |
| Connected pages | related_pages |

## What NOT to Do
- Do NOT call qmd_search more than 5 times for the same topic — diminishing returns.
- Do NOT expect people/ pages in results — they are excluded from the index.
- Do NOT panic on the first call being slow (~15s) — subsequent calls are ~1-2s.

## Trust Boundary
- QMD indexes wiki content from verified internal emails only.
- Search results may include pages with outdated status — always check the `status:` field in frontmatter.
