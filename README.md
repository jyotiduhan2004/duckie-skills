# Duckie Skills — IndiaMART Knowledge Agent

10 G-Brain-inspired skills for Duckie, IndiaMART's email-to-wiki knowledge agent. Skills inject domain-specific knowledge that LLMs don't have — IndiaMART acronyms, wiki structure, search strategies, compilation rules.

## Architecture

Skills are loaded on-demand via `load_skill(name)` — injecting context into the agent's session without sub-agents. Inspired by Google G-Brain's skill injection pattern and Perplexity's three-tier loading:

- **Tier 1:** Skill index in system prompt (~100 tokens/skill, always loaded)
- **Tier 2:** Full SKILL.md loaded on demand when triggered (~200-500 lines)
- **Tier 3:** References and scripts loaded when needed (unlimited)

## Query Skills (6) — Runtime knowledge for the query agent

| Skill | Trigger | Purpose |
|-------|---------|---------|
| `search-strategy` | Searching, unknown terms | Search tool selection, IndiaMART acronyms |
| `contact-finder` | "Who owns X?", "Who to contact?" | Ownership and contact resolution |
| `wiki-navigation` | Exploring wiki, browsing domains | Wiki structure, categories, page format |
| `qmd-usage` | Using qmd_search tool | QMD hybrid search best practices |
| `indiamart-context` | Organizational questions | Products, teams, internal tools |
| `gap-tracker` | No results found | Graceful missing-info handling |

## Compile Skills (4) — Compilation pipeline rules

| Skill | Trigger | Purpose |
|-------|---------|---------|
| `article-structure` | Creating/updating wiki pages | Wikipedia-inspired page templates |
| `content-filter` | Processing raw emails | Two-stage content filtering (regex + LLM) |
| `link-audit` | Post-compilation | Wikilink graph health checks |
| `merge-detection` | New email arrives | Page merge/create decisions |

## Folder Structure

```
duckie-skills/
├── README.md
├── RESOLVER.md          (intent → skill routing)
├── SKILL_INDEX.json     (machine-readable registry)
├── query/               (6 query-time skills)
│   ├── search-strategy/ (SKILL.md + references/ACRONYMS.md)
│   ├── contact-finder/
│   ├── wiki-navigation/
│   ├── qmd-usage/
│   ├── indiamart-context/
│   └── gap-tracker/
└── compile/             (4 compile-time skills)
    ├── article-structure/ (SKILL.md + references/TEMPLATE.md)
    ├── content-filter/    (SKILL.md + references/ + scripts/)
    ├── link-audit/        (SKILL.md + scripts/audit_graph.py)
    └── merge-detection/   (SKILL.md + references/WIKIPEDIA_RULES.md)
```

## Key Design Decisions

- **Skills teach what LLMs don't know** — not generic abilities (compare, aggregate) but IndiaMART-specific knowledge (acronyms, wiki structure, org context).
- **Single session injection** — no sub-agents, no context loss. load_skill() injects directly into the main agent's context.
- **Forward-only** — new skills apply to new compilations only, never retroactively rewrite existing pages.
- **Progressive disclosure** — heavy references gated behind "read X when Y happens" cues.

## Part of Duckie

These skills power [Duckie](https://github.com/jyotiduhan2004/IM-Hackathon), IndiaMART's email-to-wiki knowledge system. 14,619 emails → 2,379 wiki pages → 1 intelligent query agent.
