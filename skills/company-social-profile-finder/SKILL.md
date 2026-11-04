---
name: company-social-profile-finder
version: 1.0.0
author: GeminiGeorge22
description: "Which social profiles belong to this company? Resolves a name, website or Google Maps listing to its LinkedIn, X, YouTube, Instagram and Facebook handles, with confidence and evidence. Uses Apify Actor publicrecords/company-social-profile-finder through MCP; needs an Apify token (OAuth/token)."
metadata: {"nexscope":{"emoji":"🔗","category":"lead-generation"}}
---

# Company Social Media Profile Finder 🔗

Give the agent company social handles to reason from: LinkedIn, X, YouTube, Instagram and Facebook — with confidence scores and evidence — from a name, website, or Google Maps place. On the default path, Instagram and Facebook links come from the company's own site and are not platform-verified.

Disclosure: I maintain this Actor.

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

Auth: Apify OAuth or your own Apify API token; runs use your Apify account.

## What a record contains

Main fields of an entity row:

| field | meaning |
|---|---|
| `type`, `schemaVersion`, `canonicalId` | row kind + stable org id |
| `name`, `description`, `logo` | organization identity |
| `primaryDomain`, `domains[]` | website(s) with confidence + evidence |
| `places[]` | address / locality when known |
| `identifiers.<platform>` | per-platform handle: `id`, `url`, `confidence`, `confident`, `verification`, `evidence[]` |
| platforms typically present | `linkedin`, `instagram`, `x` / `twitter`, `facebook`, `youtube`, plus `github` / `wikidata` when found |

Ambiguous name queries may return a ranked `candidates` row to pick from instead of a resolved entity.

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

Optional: `inputDatasetId` + `domainField` to enrich an existing dataset in place; leave `queries` empty when using that path. Use the default `resolve` mode.

## Limits

- Organizations and places only — people queries are refused.
- Name-only queries resolve when exactly one candidate is unambiguous; otherwise you get ranked candidates.
- Confidence and verification strength come from declared + probe evidence; treat `confident: false` as provisional.
