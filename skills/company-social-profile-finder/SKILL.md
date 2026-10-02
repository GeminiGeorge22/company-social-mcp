---
name: company-social-profile-finder
version: 1.0.0
author: GeminiGeorge22
description: "Which social profiles belong to this company? Resolves a name, website or Google Maps listing to its verified LinkedIn, Instagram, X, Facebook and YouTube handles, with evidence. Uses Apify Actor publicrecords/company-social-profile-finder through MCP; needs an Apify token (OAuth/token)."
metadata: {"nexscope":{"emoji":"🔗","category":"lead-generation"}}
---

# Company Social Media Profile Finder 🔗

Give the agent verified company social handles to reason from: LinkedIn, Instagram, X, Facebook and YouTube — with confidence scores and evidence — from a name, website, or Google Maps place.

Disclosure: I maintain these Actors on Apify Store (publicrecords).

## Installation

```bash
npx skills add GeminiGeorge22/company-social-mcp --skill company-social-profile-finder -g
```

This skill calls the **Company Social Media Profile Finder** Actor (`publicrecords/company-social-profile-finder`) through the Apify MCP server:

```json
{
  "mcpServers": {
    "company-social": {
      "url": "https://mcp.apify.com/?tools=publicrecords/company-social-profile-finder",
      "headers": { "Authorization": "Bearer <APIFY_TOKEN>" }
    }
  }
}
```

Auth: Apify OAuth/token. An Apify account is free; the Actor bills via Console PPE (`actor-start`, `entity-resolved`, `identity-change` — see repo README).

## What a record contains

Field names from a live SUCCEEDED dataset item (entity row, schemaVersion 3):

| field | meaning |
|---|---|
| `type`, `schemaVersion`, `canonicalId` | row kind + stable org id |
| `name`, `description`, `logo` | organization identity |
| `primaryDomain`, `domains[]` | website(s) with confidence + evidence |
| `places[]` | address / locality when known |
| `identifiers.<platform>` | per-platform handle: `id`, `url`, `confidence`, `confident`, `verification`, `evidence[]` |
| platforms typically present | `linkedin`, `instagram`, `x` / `twitter`, `facebook`, `youtube`, plus `github` / `wikidata` when found |

Ambiguous name queries may return a ranked `candidates` row (free) instead of a charged `entity-resolved`.

## Usage

Ask the agent, for example:

- "Which social profiles belong to *apify.com*? Prefer confirmed LinkedIn and Instagram."
- "Enrich these CRM domains with LinkedIn company URLs: *stripe.com*, *notion.so*."
- "Resolve the Google Maps place for Acme Plumbing in Toronto to its Facebook and X handles."

Tool call the agent makes (`publicrecords--company-social-profile-finder`):

```json
{ "queries": ["apify.com"], "mode": "resolve" }
```

Single-shot agent/Zapier style:

```json
{ "query": "apify.com" }
```

Optional: `inputDatasetId` + `domainField` to enrich an existing dataset in place; leave `queries` empty when using that path. Buyer modes: `resolve` (default), `export`, `forget`. Publisher maintenance modes (`healthcheck`, `reverify`, `seed`, `buildSecIndex`) are not for buyer runs.

## Limits

- Organizations and places only — people queries are refused.
- Name-only queries resolve when exactly one candidate is unambiguous; otherwise you get ranked candidates (no entity charge).
- Confidence and verification strength come from declared + probe evidence; treat `confident: false` as provisional.
