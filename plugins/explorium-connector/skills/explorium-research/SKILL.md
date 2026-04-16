---
description: "Use Explorium B2B intelligence tools to research companies, enrich business data, find prospects, and track business events. Includes Web-Search-Events — an AI-powered deep scan that discovers M&As, partnerships, and deals from web sources that structured data misses. Trigger when user asks to 'research a company', 'enrich company data', 'find prospects', 'look up business info', 'company intelligence', or 'business events'."
---

# Explorium Research

Use the Explorium MCP tools to gather B2B intelligence. Available tools:

## Tools

- **match-business** — Find and match a business entity by name, domain, or other identifiers. Use this first when starting research on a company.
- **enrich-business** — Get detailed firmographic data: revenue, headcount, funding, industry, tech stack, and more. Use after matching.
- **fetch-businesses-events** — Track recent business events: funding rounds, product launches, leadership changes, expansions, layoffs.
- **fetch-prospects** — Find people/contacts at a target company by role, seniority, department.
- **enrich-prospects** — Get detailed info on specific prospects: title, contact details, background.
- **web-search** — Explorium's web search for supplementary company intelligence.

---

## Phase 1: Core Research (always run)

1. Start with **match-business** to identify the company entity
2. Use **enrich-business** to pull firmographics. Include these enrichments: firmographics, technographics, funding-and-acquisitions, workforce-trends, company-ratings, company-hierarchies. Run in parallel with step 3.
3. Use **fetch-businesses-events** for recent activity. Include all major event types: new_funding_round, new_product, new_partnership, merger_and_acquisitions, cost_cutting, increase_in_all_departments, decrease_in_all_departments, new_office, closing_office, lawsuits_and_legal_issues, company_award, outages_and_security_breaches. Set timestamp_from to 2 years ago from today.
4. If the user asked about people/contacts, use **fetch-prospects** + **enrich-prospects**
5. Present findings to the user using the output guidelines below

---

## Phase 2: Web-Search-Events (offer after Phase 1)

After presenting Phase 1 results, **always offer the user** the option to run deeper web-based event discovery. Use this exact phrasing:

> "Would you like me to run **Web-Search-Events** — a deeper scan that searches the web for business events (M&As, partnerships, major deals, leadership changes) that may not appear in structured data?"

**Only run Phase 2 if the user accepts.**

### Step 1: Multi-angle web search

Run 3 **web-search** queries in parallel, designed to surface different event types. Use the company name and domain from Phase 1's match-business result.

**Query templates** (replace variables with actual values):

1. **M&A and partnerships:**
   `"{company_name}" merger acquisition partnership deal {year_minus_2} {year_minus_1} {current_year}`

2. **Funding, expansion, and leadership:**
   `"{company_name}" OR "{domain}" funding expansion layoffs leadership CEO appointment {year_minus_1} {current_year}`

3. **Products and collaborations:**
   `"{company_name}" "{naics_description_or_industry}" new product launch collaboration joint venture {year_minus_1} {current_year}`

For query 3, use the NAICS description or LinkedIn industry category from the Phase 1 firmographics enrichment as the industry keyword.

### Step 2: Classify and extract events

From ALL search results (titles + snippets + URLs), extract every identifiable business event. For each event, extract these fields:

| Field | Description |
|---|---|
| **Event Type** | One of: Merger/Acquisition, Partnership, Funding Round, Product Launch, Leadership Change, Expansion, Contraction/Layoffs, Legal/Regulatory, Award, IPO, Other |
| **Date** | As specific as available from the snippet |
| **Summary** | One sentence describing the event |
| **Counterparty** | The other company/entity involved (if any) |
| **Deal Size** | Dollar amount if mentioned, otherwise "Undisclosed" |
| **Source** | Publication name and URL |
| **Confidence** | **High** = 2+ independent sources confirm it. **Medium** = 1 credible source (Reuters, Bloomberg, major trade pub). **Low** = single unverified or tangential mention |

### Step 3: Present structured output

**Events table:**

| Event Type | Date | Summary | Counterparty | Deal Size | Confidence | Source |
|---|---|---|---|---|---|---|

**Relationships discovered:**
List all company-to-company relationships found. Format: `Company A ↔ Company B → Relationship Type (Date)`

**Extracted metrics:**
Any quantitative data points found in search results (market size, employee counts, valuations, growth rates, etc.)

### Step 4: Gap analysis

Compare Phase 2 findings against Phase 1 structured event data. Explicitly state:
- Which events Phase 2 found that Phase 1 **missed** (this is the key value-add)
- Which events appeared in both (confirms structured data quality)
- If Phase 2 found nothing new beyond Phase 1, say so explicitly

### Phase 2 guardrails

- **Entity disambiguation is critical.** Common company names (e.g., "Circa", "Atlas", "Core") will return results for unrelated entities. Before including an event, verify the search result refers to the correct company by checking domain, industry, location, or key people against Phase 1 firmographics. Flag any uncertain matches.
- **Do not run Phase 2 automatically.** Always wait for user confirmation.
- **Do not use web-search for anything other than event discovery in Phase 2.** General research questions should use Phase 1 tools.

---

## Output guidelines (both phases)

- Lead with the most decision-relevant data (revenue, headcount, growth signals)
- Flag anything unusual or noteworthy
- Keep output concise — tables for structured data, bullets for highlights
- In Phase 2, always highlight which events were NOT found in Phase 1 structured data
