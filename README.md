# Company Social MCP: Website to LinkedIn & Social Links

<!-- mcp-name: io.github.GeminiGeorge22/company-social-mcp -->

An MCP server that tells your AI assistant which social profiles belong to a company. Give it a website domain, a company name (optionally with a city), a profile URL, a prefixed handle or a Google Maps place ID, and it returns the company's **LinkedIn, X, YouTube, Instagram and Facebook** links, each with a **confidence score** and the **evidence** behind it. Works for one company in a chat or for a whole dataset of leads.

On the default path, Instagram and Facebook links are read from the company's own website and are not platform-verified; each link carries its verification status. Companies and places only: lookups for people are refused.

Built on public web pages and open data, via the Apify Actor [Company Website to LinkedIn & Social Media Links](https://apify.com/publicrecords/company-social-profile-finder). Not affiliated with LinkedIn, Meta, X Corp, Google or YouTube.

- Apify Store: https://apify.com/publicrecords/company-social-profile-finder
- MCP server: https://mcp.apify.com?tools=publicrecords/company-social-profile-finder

## Example prompts

- "Which social profiles belong to stripe.com?"
- "Find the LinkedIn and X accounts for these domains: notion.so, linear.app, vercel.com. Only confident links."
- "Is https://www.instagram.com/acme really the official account of Acme Plumbing, Toronto?"
- "Enrich my Google Maps leads dataset with each business's social links."

## Tool

| Tool | Input | Output |
| --- | --- | --- |
| `publicrecords--company-social-profile-finder` | `queries` (domains, company names, profile URLs, handles such as `ig:acme`, Maps place IDs) or a single `query`; or `inputDatasetId` (+ `domainField`, `maxRows`) to enrich an existing dataset; `outputFormat`, `confidentThreshold`, `forceRefresh`, `maxConcurrency` | One row per company: resolved organization, LinkedIn / X / YouTube / Instagram / Facebook links with confidence, a verified flag and (in `full` format) the evidence trail. Ambiguous names return a ranked `candidates` row to pick from |

The tool exposes the Actor's everyday input fields; every other Actor field keeps its default. The tool schema is in [`tools.json`](tools.json), generated from the Actor's public input schema and checked for drift daily.

## MCP endpoint

```
https://mcp.apify.com?tools=publicrecords/company-social-profile-finder
```

Transport: Streamable HTTP (hosted by Apify, see https://docs.apify.com/mcp).

## Auth

Sign in with Apify OAuth when your client prompts you, or send the header
`Authorization: Bearer <APIFY_TOKEN>` where `<APIFY_TOKEN>` is your own Apify API token.
Never commit a real token.

## Client setup

### Claude

Claude Code:

```
claude mcp add --transport http company-social "https://mcp.apify.com?tools=publicrecords/company-social-profile-finder"
```

Claude Desktop / claude.ai: Settings → Connectors → Add custom connector, and paste the MCP endpoint URL above.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "company-social": {
      "url": "https://mcp.apify.com?tools=publicrecords/company-social-profile-finder"
    }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "company-social": {
      "type": "http",
      "url": "https://mcp.apify.com?tools=publicrecords/company-social-profile-finder"
    }
  }
}
```

To use a token instead of OAuth, add `"headers": { "Authorization": "Bearer <APIFY_TOKEN>" }` next to `"url"`.

---

Disclosure: I maintain this Actor.

## Local stdio server (open source)

This repo also ships a small MCP server in [`src/`](src/) for clients that only run local stdio servers. It answers `initialize` and `tools/list` locally from [`tools.json`](tools.json), which is generated from the Actor's public input schema and checked for drift in CI and daily, so any client or directory can inspect the tools with no token. `tools/call` is forwarded to the hosted Apify MCP endpoint above with **your own** `APIFY_TOKEN`; without a token a call returns a clear error instead of running anything. The token is only sent in the `Authorization` header and is never logged.

```bash
npx -y github:GeminiGeorge22/company-social-mcp
```

```json
{
  "mcpServers": {
    "company-social": {
      "command": "npx",
      "args": ["-y", "github:GeminiGeorge22/company-social-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or from a clone:

```bash
git clone https://github.com/GeminiGeorge22/company-social-mcp.git
cd company-social-mcp && npm install
APIFY_TOKEN=YOUR_APIFY_TOKEN node src/server.js
```

Optional env: `APIFY_ACTORS` (comma-separated subset of the Actors above) and `APIFY_MCP_URL` (full endpoint override).

Tests run against an in-memory mock server, with no network and no token: `npm test`. Requires Node.js 18+.

### Python (uvx/pip)

A matching Python version of this server lives in [`python/`](python/), built on the official `mcp` Python SDK: same tool, same `APIFY_TOKEN`, same hosted endpoint. Requires Python 3.10+.

```bash
uvx company-social-mcp
```

```json
{
  "mcpServers": {
    "company-social": {
      "command": "uvx",
      "args": ["company-social-mcp"],
      "env": { "APIFY_TOKEN": "YOUR_APIFY_TOKEN" }
    }
  }
}
```

Or with pip: `pip install company-social-mcp`, then run `company-social-mcp`. Until the first PyPI release, run it from GitHub: `uvx --from "git+https://github.com/GeminiGeorge22/company-social-mcp#subdirectory=python" company-social-mcp`. Python tests: `cd python && pip install -e ".[test]" && env -u APIFY_TOKEN python -m pytest`.

## Agent skill

[`skills/company-social-profile-finder/SKILL.md`](skills/company-social-profile-finder/SKILL.md) is a drop-in skill file that tells an agent when and how to call the tool.

## See also

- [google-ads-transparency-mcp](https://github.com/GeminiGeorge22/google-ads-transparency-mcp): Competitor Google ads from the Ads Transparency Center: new, running and stopped.
- [ats-jobs-mcp](https://github.com/GeminiGeorge22/ats-jobs-mcp): New job postings from Greenhouse, Lever, Ashby and SmartRecruiters boards.
- [gov-tenders-mcp](https://github.com/GeminiGeorge22/gov-tenders-mcp): Government tenders, grants and contract awards.
- [spotify-play-count-mcp](https://github.com/GeminiGeorge22/spotify-play-count-mcp): Spotify play counts, monthly listeners and followers.

## License

MIT
