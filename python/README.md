# company-social-mcp (Python)

<!-- mcp-name: io.github.GeminiGeorge22/company-social-mcp -->

Python version of the [company-social-mcp](https://github.com/GeminiGeorge22/company-social-mcp) stdio MCP server, built on the official [`mcp`](https://pypi.org/project/mcp/) Python SDK. Same tool, same `APIFY_TOKEN` env var, same hosted endpoint as the Node.js server.

Finds a company's LinkedIn, X, YouTube, Instagram and Facebook profiles from a website domain, company name, profile URL, handle or Google Maps place ID, each link with a confidence score and evidence. On the default path, Instagram and Facebook links come from the company's own website and are not platform-verified.

It answers `initialize` and `tools/list` locally from the bundled `tools.json` (no token, no network). `tools/call` is forwarded to Apify's hosted MCP endpoint (`mcp.apify.com`) with **your own** `APIFY_TOKEN`, so each run uses your Apify account. Without a token a call returns a clear error instead of running anything. The token is only sent in the `Authorization` header and is never logged.

## Install

Requires Python 3.10+. With [uv](https://docs.astral.sh/uv/):

```bash
uvx company-social-mcp
```

With pip:

```bash
pip install company-social-mcp
company-social-mcp
```

To run the latest unreleased code from GitHub: `uvx --from "git+https://github.com/GeminiGeorge22/company-social-mcp#subdirectory=python" company-social-mcp`.

MCP client config:

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

Optional env: `APIFY_ACTORS` (comma-separated subset of the Actors) and `APIFY_MCP_URL` (full endpoint override).

## Tests

Tests use an in-process mock server, with no network and no token. They fail if `APIFY_TOKEN` is set.

```bash
cd python && pip install -e ".[test]"
env -u APIFY_TOKEN python -m pytest
```

`python scripts/build_tools.py --check` compares the bundled `tools.json` with each Actor's live public input schema and exits 1 on any mismatch.
