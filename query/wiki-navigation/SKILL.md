---
name: wiki-navigation
description: "Explains IndiaMART wiki structure, categories, domains, page formats, and wikilink conventions. Load when exploring the wiki structure, browsing domains, understanding page organization, or following wikilinks between pages."
metadata:
  type: query
  version: 1.0
---

## Wiki Categories (5 content types)

| Category | Pages | Contains |
|----------|-------|----------|
| `topics/` | ~434 | Concept pages: features, initiatives, launches. The bulk of the wiki. |
| `systems/` | ~110 | Products and platforms (PhotoSearch, GLAdmin, WhatsApp 9696). |
| `people/` | ~628 | Person pages with email and "Appears in" section. Reference only. |
| `policies/` | ~1 | Mostly empty. Policies lazy-created. |
| `decisions/` | ~1 | Mostly empty. Decisions lazy-created. |

## Domain Hubs (8 domains)

Read domain hubs first for domain-scoped questions: `cat('domains/{name}')`.

| Domain | Focus |
|--------|-------|
| buyer-experience | BuyLeads, buyer chat, search UX |
| seller-experience | Seller tools, compliance, dashboard |
| marketplace-discovery | MCAT, ISQ, photo search, ranking |
| platform-reliability | GKE, DB ops, API framework |
| trust-safety | KYC, GST, fraud, moderation |
| ai-automation | CrashAgent, chatbots, AI tools |
| growth-monetization | Ads, exports, affiliates, tenders |
| engineering-productivity | CI/CD, code quality, testing |

## Page Format

Every page has YAML frontmatter:
```yaml
title: "Page Title"
page_type: topic|system|person|domain
status: active|superseded|archived
domain: buyer-experience
owner: '[[person-slug]]'
source_threads: ["thread-id"]
related: ["[[topic/other-page]]"]
```

Body structure: lead paragraph → ## sections (Current State, Key Details, Open Questions, Sources).

## Wikilink Format
- `[[seller-isq]]` — topic page
- `[[systems/photosearch]]` — system page
- `[[people/amit-agarwal-indiamart-com]]` — person page
- `[[domains/ai-automation]]` — domain hub

## Special Pages
- `index.md` — Home page with domain overview
- `changes.md` — Activity feed of recently updated pages
- `glossary.md` — 110+ acronyms (auto-generated)

## What NOT to Do
- Do NOT read every page in a domain — use `head()` to scan, then `cat()` only relevant pages.
- Do NOT assume `policies/` or `decisions/` have content — they are mostly empty.
- Do NOT confuse person page slugs with display names — slugs are email-based (`amit-agarwal-indiamart-com`).

## Person Page Slugs
Person slugs are email-based, not display names: `amit-agarwal-indiamart-com` (not "Amit Agarwal"). Use `keyword_search(name)` to find the slug. Person pages have an `email:` field and an `## Appears in` section listing their projects.

## Coverage Limitation
This wiki covers **January to mid-February 2026** IndiaMART launch emails only. Topics outside this window may not exist.
