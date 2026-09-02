# Namefi plugin for Antigravity

Search, register, and manage domains and their DNS from the
[Antigravity CLI](https://antigravity.google/docs/cli/plugins) (`agy`).

## Install

Antigravity installs plugins from a local path — there is no marketplace or
remote install yet — so clone this repo first:

```bash
git clone https://github.com/d3servelabs/namefi-antigravity-plugins.git
agy plugin install ./namefi-antigravity-plugins/plugins/namefi
```

Then `agy plugin list` to confirm it is enabled. `agy plugin disable namefi`,
`agy plugin enable namefi`, and `agy plugin uninstall namefi` manage it
afterwards.

## What's in the plugin

| Component | What it does |
| --- | --- |
| MCP server `namefi` | The hosted Namefi MCP server (`https://api.namefi.io/mcp`) — availability search, registration + order polling, DNS records, domain config, outbound lead-finding |
| Skill `/namefi-domains` | The MCP-first flow: confirm before spending, register → poll order, cart hand-off for human checkout, and the REST fallback table |
| Skills `/namefi-dyndns*` | Point a domain at a machine with no static IP — diagnose VPS vs NAT, carrier NAT, and drive the `namefi` CLI, ddclient, or a plain cron `curl` |
| Skill `/namefi-https-and-routing` | Put Caddy or Traefik in front of those services so the domain serves real HTTPS |

Skills become slash commands, so `/namefi-domains` is how you reach the first one.

## Authentication

Every Namefi call is authenticated, including availability search — there is no
anonymous access.

- **OAuth (default, no setup):** the server implements OAuth 2.1 + PKCE with
  dynamic client registration, so nothing is pre-registered. Authorize it in the
  browser when Antigravity asks; the token lands in
  `~/.gemini/antigravity/mcp_oauth_tokens.json` and refreshes automatically.
- **API key (headless / CI):** generate one at <https://namefi.io/api-key> and add
  a header to the `namefi` entry in `~/.gemini/config/mcp_config.json` (global) or
  `.agents/mcp_config.json` (workspace):

  ```json
  {
    "mcpServers": {
      "namefi": {
        "serverUrl": "https://api.namefi.io/mcp",
        "headers": { "x-api-key": "nfk_YOUR_KEY" }
      }
    }
  }
  ```

The bundled `mcp_config.json` deliberately ships **without** an API-key header —
OAuth covers the default path, and a placeholder header would break the server for
everyone who never filled it in.

## Layout

```
plugins/namefi/
├── plugin.json                          # required manifest (name + description)
├── mcp_config.json                      # bundled Namefi MCP server
└── skills/
    ├── namefi-domains/SKILL.md
    ├── namefi-dyndns/SKILL.md           # the dyndns front door
    │   └── references/inbound-reachability.md   # shared by all the dyndns skills
    ├── namefi-dyndns-with-cli/SKILL.md
    ├── namefi-dyndns-with-ddclient/SKILL.md
    ├── namefi-dyndns-without-tooling/SKILL.md
    └── namefi-https-and-routing/SKILL.md
```

The dyndns skills reach the shared reference as
`../namefi-dyndns/references/inbound-reachability.md`, so they must stay flat
siblings — copy them as a set.

## Related

The same plugin for other agents:
[Claude Code](https://github.com/d3servelabs/namefi-claude-plugins),
[Codex](https://github.com/d3servelabs/namefi-codex-plugins),
[ZCode](https://github.com/d3servelabs/namefi-zcode-plugins). All wrap the same
hosted MCP server; the manifests and per-platform auth instructions differ.

## License

MIT
