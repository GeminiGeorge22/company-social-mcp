# company-social-mcp

Drop-in **company social profile resolution** for **sales and marketing software** — CRMs, lead-gen tools, enrichment pipelines, and AI assistants — via [Apify’s hosted MCP](https://mcp.apify.com) or the Store Actor directly.

Resolves a **company name, website, or Google Maps listing** to its verified **LinkedIn, Instagram, X, Facebook and YouTube** handles, with evidence. **Manifest-only repo — no Actor source.**

**Disclosure:** I maintain these Actors on Apify Store (`publicrecords`). This repo documents MCP / client config only.

## MCP pin

```
https://mcp.apify.com/?tools=publicrecords/company-social-profile-finder
```

| Actor | Store / status | Role |
|---|---|---|
| **Company Social Media Profile Finder** | [publicrecords/company-social-profile-finder](https://apify.com/publicrecords/company-social-profile-finder) (listed) | Resolve name / website / Maps place → LinkedIn, Instagram, X, Facebook, YouTube (+ evidence) |

Use either path:

- **Apify MCP** (Claude Desktop, Cursor, or any MCP client) with the pin above
- **Store Actor** directly in Apify Console / API for pipelines, CRMs, and dashboards

Auth: your own Apify token in the client when prompted. Runs are billed on your Apify account.

## Pricing (Console PPE, FREE tier base, live 2026-10-02)

| Event | USD |
|---|---|
| `actor-start` | $0.005 |
| `entity-resolved` (primary) | $0.02 |
| `identity-change` | $0.01 |

Measured proof: `apify.com` → run `dqgG9vmZuLevEMQ3T` — 5 confident / 4 confirmed handles (2026-09-28).

## Tools

Live `tools/list` against the pin URL (Bearer Apify token). Tool names exposed:

| Tool name | Role |
|---|---|
| `publicrecords--company-social-profile-finder` | Run Company Social Media Profile Finder (`publicrecords/company-social-profile-finder`) |
| `get-actor-run` | Get detailed information about a specific Actor run |
| `get-dataset-items` | Read items from a run dataset |
| `get-key-value-store-record` | Read a key-value store record |
| `abort-actor-run` | Abort a running Actor run |

## Claude Desktop

Add to `claude_desktop_config.json` (macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "company-social": {
      "command": "npx",
      "args": [
        "-y",
        "@apify/actors-mcp-server",
        "--actors",
        "publicrecords/company-social-profile-finder"
      ],
      "env": {
        "APIFY_TOKEN": "YOUR_APIFY_TOKEN"
      }
    }
  }
}
```

Or point the Apify remote MCP URL (with the pin above) if your Claude Desktop build supports remote MCP servers.

## Cursor

In Cursor MCP settings / `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "company-social": {
      "url": "https://mcp.apify.com/?tools=publicrecords/company-social-profile-finder"
    }
  }
}
```

Sign in with Apify when Cursor prompts.

## Example prompts

### 1) Resolve a company website

**Prompt:** Which social profiles belong to `apify.com`?

**Expected:** LinkedIn / Instagram / X / Facebook / YouTube handles with confidence + evidence (see proof run `dqgG9vmZuLevEMQ3T`).

### 2) Enrich a CRM row

**Prompt:** Resolve social handles for these domains: `stripe.com`, `notion.so`. Return confident LinkedIn and X only.

Tool call the agent makes (`publicrecords--company-social-profile-finder`):

```json
{ "queries": ["stripe.com", "notion.so"], "mode": "resolve" }
```

## What this repo is not

- Not Actor implementation / Docker / truth sets
- Not a substitute for Apify Console billing screens
- Not affiliated with LinkedIn, Meta, X Corp, Google, or YouTube

## License

MIT — applies to this documentation and config only.

Published by the publicrecords maintainer on Apify.
