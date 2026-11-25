# Namefi: Instant Search

Brainstorm startup, business, and brand names with Antigravity and check domain
availability as you go. Inspect public DNS records, including website and email
configuration. No account, sign-in, or API key is needed.

## Install locally

```bash
git clone https://github.com/d3servelabs/namefi-antigravity-plugins.git
agy plugin validate --strict ./namefi-antigravity-plugins/plugins/namefi
agy plugin install ./namefi-antigravity-plugins/plugins/namefi
agy plugin list
```

The plugin keeps the internal name `namefi`. Manage it with
`agy plugin disable namefi`, `agy plugin enable namefi`, or
`agy plugin uninstall namefi`.

## What's included

The plugin connects to the public `https://api.namefi.io/mcp-instant` endpoint.
It bundles two read-only MCP tools and the existing Namefi logo, with no skills,
agents, rules, or hooks. Registration, payments, DNS changes, dynamic DNS, and
HTTPS routing from the earlier package are no longer included.

See the [plugin overview](plugins/namefi/README.md) for tool details, examples,
and support links.

## Layout

```text
plugins/namefi/
├── plugin.json
├── mcp_config.json
├── README.md
└── assets/namefi-icon.png
```

The manifest follows the [Antigravity plugin format](https://antigravity.google/docs/build-plugins).
The package is prepared for local installation and Marketplace submission;
this repository does not claim that a Marketplace listing is published.

## License

MIT
