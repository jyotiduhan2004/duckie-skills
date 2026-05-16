---
name: contact-finder
description: "Resolves ownership and contact information for IndiaMART projects, systems, and teams. Use when the user asks 'who should I reach out to', 'who owns X', 'who developed Y', 'who is responsible for Z', or needs to find a specific person's email."
metadata:
  type: query
  version: 1.0
---

## How to Find Contacts

### Step 1: Check the topic/system page
1. Run `cat(slug)` to read the page.
2. Check `owner:` field in YAML frontmatter — this is the primary contact.
3. Scan body for person mentions: `[[people/name-indiamart-com]]`.
4. Check `source_threads:` — original email thread authors are key contacts.

### Step 2: Read the person page
1. Person slugs are email-based: `amit-agarwal-indiamart-com` (not display names).
2. Run `keyword_search(person_name)` to find the slug.
3. Person pages contain:
   - `email:` field with @indiamart.com address
   - `## Appears in` section listing all their projects

### Step 3: For team-level contacts
1. Read domain hub: `cat('domains/seller-experience')`.
2. People appearing in 3+ pages within the same domain are likely team leads.

## Worked Example

**User asks:** "Who developed VANI 2.0?"

1. Run `qmd_search("VANI 2.0")` → finds `seller-vani` system page
2. Run `cat("seller-vani")` → owner field shows `[[chittresh-lohani-indiamart-com]]`
3. Run `cat("chittresh-lohani-indiamart-com")` → email: chittresh.lohani@indiamart.com, appears in 4 AI projects
4. Answer: "VANI 2.0 was developed by Chittresh Lohani (chittresh.lohani@indiamart.com). They appear in 4 AI-automation projects including WhatsApp 9696 and seller VANI."

## What to Tell the User
- Always provide the email address from the person page.
- Mention their role context: "X appears in 5 seller-experience projects including [top 3]."
- If owner field missing: "No explicit owner listed, but [person] is a contributor."
- If no person found: "I couldn't find a specific contact. Check the domain hub for related team members."

## What NOT to Do
- Do NOT guess email addresses — only use addresses found in person pages.
- Do NOT confuse `owner:` (DRI) with `source_threads:` authors (email thread participants).
- Do NOT report people from unrelated projects just because they share a domain.
- STOP after checking the topic page + 2 person pages. If no contact found, say so.

## Trust Boundary
- Email addresses are internal @indiamart.com addresses from verified launch emails.
- Never fabricate person details or infer reporting relationships not in the wiki.
