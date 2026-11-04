# Changelog

All notable changes to this project.

## [0.1.0] - 2026-11-03

- Open-source stdio MCP server (Node.js, plus a matching Python package `company-social-mcp` in `python/`) for the Company Website to LinkedIn & Social Media Links Actor. Until now this repo was a manifest-only pin for the hosted endpoint.
- `initialize` and `tools/list` work without a token: the tool is served from a checked-in `tools.json` built from the Actor's public input schema (`npm run build:tools`), exposing the everyday input fields listed in `package.json` (`mcpProxy.inputs`).
- `tools/call` goes to Apify's hosted MCP gateway with your own `APIFY_TOKEN`; without a token it returns a clear error.
- Tests run with no token (an env guard fails the suite if `APIFY_TOKEN` is set); CI and a daily workflow fail on drift between `tools.json` and the live public input schema.
- `server.json` 1.0.1 for the official MCP Registry (same name, Streamable HTTP remote, Apify Store website); publish workflow on GitHub OIDC; PyPI trusted-publishing workflow; Dockerfile, glama.json, SECURITY.md and Dependabot config.
- README rewritten: tool, endpoint, client setup, stdio and uvx install.
