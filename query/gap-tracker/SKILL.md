---
name: gap-tracker
description: "Handles missing information scenarios gracefully. Use when a search returns no results, the wiki doesn't cover the user's topic, or the answer is incomplete. Guides the agent on what to tell the user and how to log gaps."
metadata:
  type: query
  version: 1.0
---

## When Information is Not Found

### Step 1: Acknowledge honestly
Tell the user: "I couldn't find specific information about [topic] in the wiki."

### Step 2: State coverage
"The wiki covers January to mid-February 2026 IndiaMART launch emails from marketplacelaunch@indiamart.com."

### Step 3: Offer what you found
Suggest related pages discovered during search. Even partial answers are better than nothing.

### Step 4: Explain why it might be missing
- Topic launched after mid-February 2026
- Covered in IM Reports (separate mailing list, not ingested)
- Too granular (e.g., specific Jira ticket details)

### Step 5: Suggest next steps
Check if any person pages mention the topic area and suggest who the user could reach out to.

## Worked Example

**User asks:** "What's the status of the new payment gateway migration?"

1. Run `qmd_search("payment gateway migration")` → 0 relevant results
2. Run `keyword_search("payment")` → finds `buyer-payments` but no migration page
3. Run `cat("buyer-payments")` → no mention of gateway migration
4. Answer: "I couldn't find information about a payment gateway migration in the wiki. The wiki covers Jan-mid Feb 2026 launch emails. This may have been announced after that period or in a different mailing list. The closest page is [[buyer-payments]] — you could check with the owner listed there."

## Common Gaps in This Wiki
- **March 2026 onwards** — not yet compiled
- **IM Reports** — separate mailing list, not ingested
- **Detailed metrics** — some pages lack performance data
- **Team org charts** — contributions visible, reporting structure is not
- **Jira/ticket details** — mentioned in emails but not tracked separately

## What NOT to Do
- Do NOT fabricate answers when the wiki doesn't have the information.
- Do NOT say "the information doesn't exist" — say "it's not in this wiki."
- Do NOT skip suggesting related pages — partial context is valuable.
- STOP searching after confirming the gap. Do not loop endlessly.

## Trust Boundary
- Gap logging is automatic and internal. The user does not need to take action.
- Never invent information to fill a gap — honesty builds trust.
