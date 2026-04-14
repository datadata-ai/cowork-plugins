# Explorium Connector

B2B intelligence plugin powered by Explorium. Research companies, enrich business data, find prospects, and track business events.

## What's new in v0.2.0: Web-Search-Events

**Web-Search-Events** is an AI-powered deep event discovery layer that runs on top of Explorium's existing web-search tool. It finds business events (M&As, partnerships, major deals, leadership changes) that structured data pipelines often miss — especially for private companies.

How it works:
1. **Phase 1** runs the standard Explorium research workflow (match → enrich → structured events)
2. After Phase 1, the user is offered **Phase 2: Web-Search-Events**
3. Phase 2 runs multiple targeted web searches, then uses LLM classification to extract structured events from the results
4. A gap analysis compares web-discovered events against structured data, highlighting what was missed

### Why this matters

Structured B2B databases are great at tracking public company events, funding rounds, and corporate hierarchies. But they frequently miss:
- Mergers between private companies
- Operational partnerships not filed with regulators
- Industry-specific deals covered only by trade press

Web-Search-Events closes this gap at near-zero marginal cost.

## Components

- **Skill**: `explorium-research` — Two-phase company research workflow
- **MCP Server**: Explorium SSE endpoint with API key auth

## Usage

Trigger phrases:
- "Research [company]"
- "Enrich company data for [domain]"
- "Find prospects at [company]"
- "What's new with [company]"
- "Company intelligence on [name]"

## Setup

The MCP server connects to `https://mcp.explorium.ai/sse` using an API key. Configure your key in `.mcp.json`.

**Important:** You need your own Explorium API key. Get one at [explorium.ai](https://www.explorium.ai/).
