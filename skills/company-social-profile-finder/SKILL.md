---
name: company-social-profile-finder
description: "Resolves a company name, website, or Google Maps listing to LinkedIn, X, YouTube, Instagram, and Facebook profiles, with confidence scores and evidence. Use when the user asks which social profiles belong to a company, needs social handles for CRM or lead enrichment, or wants a company's LinkedIn or other social URLs. Runs the Apify Actor publicrecords/company-social-profile-finder through MCP on the user's own Apify account."
license: MIT
metadata:
  author: GeminiGeorge22
  version: "1.1.0"
---

# Company social profile finder

Looks up a company's LinkedIn, X, YouTube, Instagram, and Facebook profiles from a name, website, or Google Maps place, and returns confidence scores and evidence with each handle.

On the default path, Instagram and Facebook links come from the company's own site and are not platform-verified.

The Actor is [publicrecords/company-social-profile-finder](https://apify.com/publicrecords/company-social-profile-finder) on Apify Store.

## Authentication

Runs on **the user's own Apify account**.

- Use an API token the user created in their own Apify Console, or sign in with Apify OAuth when the MCP client prompts.
- Put that token only in the client's MCP config or environment as `APIFY_TOKEN` (or as `Authorization: Bearer <token>` on the hosted endpoint).
- Never use a shared, publisher, or third-party token. Never paste a token into chat.
- `tools/list` works without a token. `tools/call` needs the user's token and runs on that user's Apify account.

## Connect the MCP server

Hosted (Streamable HTTP):

```json
{
  "mcpServers": {
    "company-social": {
      "url": "https://mcp.apify.com/?tools=publicrecords/company-social-profile-finder",
      "headers": { "Authorization": "Bearer <YOUR_APIFY_TOKEN>" }
    }
  }
}
```

Local stdio (open-source proxy in this repo), with the user's token in env:

```json
{
  "mcpServers": {
    "company-social": {
      "command": "npx",
      "args": ["-y", "github:GeminiGeorge22/company-social-mcp"],
      "env": { "APIFY_TOKEN": "<YOUR_APIFY_TOKEN>" }
    }
  }
}
```

Replace `<YOUR_APIFY_TOKEN>` with the user's own token. A Python package `company-social-mcp` is also available (`uvx` / `pip`).

## What a record contains

| field | meaning |
|---|---|
| `type`, `schemaVersion`, `canonicalId` | row kind and stable organization id |
| `name`, `description`, `logo` | organization identity |
| `primaryDomain`, `domains[]` | website(s) with confidence and evidence |
| `places[]` | address or locality when known |
| `identifiers.<platform>` | per-platform handle: `id`, `url`, `confidence`, `confident`, `verification`, `evidence[]` |
| platforms typically present | `linkedin`, `instagram`, `x` / `twitter`, `facebook`, `youtube`, plus `github` / `wikidata` when found |

Ambiguous name queries may return a ranked `candidates` row to pick from instead of a resolved entity.

## Usage

Example prompts:

- "Which social profiles belong to apify.com? Prefer confirmed LinkedIn and Instagram."
- "Enrich these CRM domains with LinkedIn company URLs: stripe.com, notion.so."
- "Resolve the Google Maps place for Acme Plumbing in Toronto to its Facebook and X handles."

Tool name: `publicrecords--company-social-profile-finder`.

```json
{ "queries": ["apify.com"], "mode": "resolve" }
```

Single-value form:

```json
{ "query": "apify.com" }
```

Optional: `inputDatasetId` plus `domainField` to enrich an existing dataset in place; leave `queries` empty on that path. Use the default `resolve` mode unless the user asks otherwise.

## Limits

- Organizations and places only. People queries are refused.
- Name-only queries resolve when exactly one candidate is unambiguous; otherwise the tool returns ranked candidates.
- Treat `confident: false` as provisional. Confidence comes from declared and probe evidence.
