---
name: expansion-upsell-intelligence
description: "Generates a Strategic Expansion Brief for a single target account by combining Explorium expansion signals with seller context. Trigger when user asks to 'run expansion brief', 'find upsell opportunities for [company]', 'expansion intelligence', 'check expansion signals for [company]', or 'upsell brief'."
---

# Expansion Trigger & Upsell Intelligence

Generate a Strategic Expansion Brief for a target account by combining Explorium event signals with seller context and web research.

---

## Phase 1: Gather Seller Context

The skill needs to understand what is being sold before it can generate a useful brief. Work through these sources in order — stop as soon as you have enough context (product description, ICP, and at least one offering/tier):

### Step 1: Check CLAUDE.md
Look for a CLAUDE.md in the current workspace. If it contains product/service description, ICP, tiers, or value propositions — extract and use that. Note what was found.

### Step 2: Ask for Documents
If CLAUDE.md didn't provide enough, ask the user:

> "To generate a sharp, specific brief I need to understand what you're selling. Do you have any materials I can read — a pitch deck, one-pager, whitepaper, or product overview? You can upload them directly."

If documents are provided, extract:
- Product/service description
- Target customer (size, industry, role)
- Key problems solved
- Tiers, packages, or offering levels
- Key differentiators or value props

### Step 3: Fallback Questions
Only if steps 1 and 2 didn't yield enough context, ask these targeted questions (only for what's still missing):

- What does your product do? (1-2 sentences)
- Who is your ideal customer? (company size, industry, buyer role)
- What are the top 2-3 problems your product solves?
- What are your tiers or packages? (e.g. Starter / Pro / Enterprise)

### Seller Profile Confirmation
Summarize the extracted seller context in a short "Seller Profile" block and confirm with the user before proceeding:

```
**Seller Profile**
Product: [what it does]
ICP: [ideal customer]
Key Problems Solved: [list]
Offerings: [tiers or packages]
```

Ask: "Does this look right? Anything to correct before I run the research?"

---

## Phase 2: Identify the Target Account

Ask the user:

> "Which company should I run this against? You can give me a name, domain, or both."

Use `match-business` to resolve the company entity, then confirm the match with the user before proceeding.

---

## Phase 3: Pull Expansion Signals

Use `fetch-businesses-events` for the matched company. Filter for:
- Event types: `new_office`, `new_product`, `new_partnership`, `new_funding_round`
- `timestamp_from`: 180 days ago from today

If no matching events are found, inform the user and stop. Suggest trying a broader time range or a different company if relevant.

---

## Phase 4: Contextualize via Web

For each event found, run web searches in parallel to surface:
- The specific region, market, or strategic rationale behind the expansion
- Job postings associated with the new office or product line (reveals hiring intent and operational focus)
- Any press coverage or announcements that add context

---

## Phase 5: Generate the Expansion Brief

Produce one **Expansion Brief** per significant event found. Use the Seller Profile from Phase 1 to make every field specific and actionable — never generic.

```
**Account:** [Company Name]
**Expansion Event:** [Event type] — [Location/Product/Partner] (Detected via Explorium, [Date])
**Strategic Mandate:** [Why they expanded — market rationale found via web search]
**Operational Gap:** [New problem this expansion creates that your product solves — be specific]
**Upsell Recommendation:** [Specific tier, feature, or package from the Seller Profile — explain why it fits]
**Outreach Hook:**
"[2-line message: genuine congratulations on the milestone, natural transition into the pitch]"
```

If multiple events are found, produce a brief for each and summarize with a recommended priority order.

---

## Guidelines

- **Specificity is everything.** Vague operational gaps and upsell angles defeat the purpose. Every field must connect directly to the expansion event AND the seller's actual offerings.
- **Use job postings as evidence.** If the company is hiring 10 compliance engineers at the new office, that's proof of the operational gap — cite it.
- **Outreach Hook tone:** Warm, human, non-salesy. Lead with genuine acknowledgment of the milestone. The pitch should feel like a natural "by the way" not a pivot.
- **Seller Profile drives everything.** If the Seller Profile doesn't have enough detail to generate a specific upsell recommendation, go back and ask rather than defaulting to generic language.
