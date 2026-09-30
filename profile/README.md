<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/logo-dark.png">
    <img src="/assets/logo.png" alt="ThinkWatch" width="480">
  </picture>
</p>

<h3 align="center">AI API and MCP gateways</h3>

<p align="center">
  <a href="https://thinkwat.ch/">Website</a> ·
  <a href="https://thinkwat.ch/docs/">Documentation</a> ·
  <a href="https://github.com/ThinkWatchProject/.github/blob/main/SECURITY.md">Security</a>
</p>

ThinkWatch places a gateway between AI clients and model providers, so that
every request is routed, inspected and recorded in one place. It comes in
three products that share one engine.

## ThinkWatch Lite

A desktop app that runs a local gateway for Claude Code, Codex and other AI
clients. Clients connect once; changing upstreams or models afterwards needs
no change to the client.

- **Connect once, switch freely.** Seven clients are set up in one step, with
  the change previewed and the original file backed up.
- **Security.** API keys can be replaced before a request leaves the machine,
  and dangerous tool calls can be cut off. MCP servers, skills, hooks and
  client configuration are scanned for hidden characters and prompt injection.
- **Every request traceable.** Each request records the rule it matched, every
  failover attempt and its cost, and can be replayed against another upstream.
- **Honest cost.** Estimates are marked as estimates, and unpriced requests are
  counted separately rather than as zero.

macOS, Windows and Linux · MIT ·
[Download](https://thinkwat.ch/lite/#install) ·
[ThinkWatch-Lite](https://github.com/ThinkWatchProject/ThinkWatch-Lite)

```bash
brew install --cask thinkwatchproject/tap/thinkwatch-lite
```

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/assets/lite-overview-dark.png">
  <img src="/assets/lite-overview-light.png" alt="The ThinkWatch Lite overview page: tokens, cost and requests for the last seven days compared with the seven days before, a trend chart by model with periods of failures marked, and usage by model">
</picture>

## ThinkWatch Enterprise

A self-hosted AI API and MCP gateway for organizations, with a web console.
It is the single entry point for a company's model requests and MCP tool calls.

- **Access control.** Single sign-on through any OIDC provider, role-based
  access and virtual API keys with their own scopes and expiry.
- **Per-user MCP identity.** Upstream MCP servers receive the calling user's
  own token instead of a shared service account.
- **Limits and budgets.** Request and token limits and token budgets per user,
  key, provider or MCP server.
- **Audit and cost.** Every request and tool call is logged, with usage and
  cost analytics.

Docker Compose or Kubernetes · Business Source License 1.1 ·
[Quick start](https://github.com/ThinkWatchProject/ThinkWatch#quick-start) ·
[ThinkWatch](https://github.com/ThinkWatchProject/ThinkWatch)

## ThinkWatch Core

The MIT-licensed gateway engine: Rust crates and the `twcore` binary. It runs
inside ThinkWatch Lite, or on its own as a gateway on a Linux server that Lite
manages remotely over an encrypted channel. ThinkWatch Enterprise builds on
four of its crates.

```bash
curl -fsSL https://raw.githubusercontent.com/ThinkWatchProject/ThinkWatch-Core/main/scripts/install.sh | sudo sh
```

[Running core on a server](https://github.com/ThinkWatchProject/ThinkWatch-Core/blob/main/docs/server.md) ·
[ThinkWatch-Core](https://github.com/ThinkWatchProject/ThinkWatch-Core)

## Choosing a product

| Need | Product |
|---|---|
| One developer's AI clients: switching, security, request records | Lite |
| A gateway on a Linux server, managed from the desktop | Core with Lite |
| AI and MCP access for a team: SSO, permissions, audit, budgets | Enterprise |

Lite and Core are MIT. Enterprise is free for non-production use and for
production use below the monthly thresholds in
[LICENSING.md](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSING.md);
[thinkwat.ch/license](https://thinkwat.ch/license/) covers commercial licenses.
