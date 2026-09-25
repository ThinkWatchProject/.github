<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/logo-dark.png">
    <img src="/assets/logo.png" alt="ThinkWatch" width="480">
  </picture>
</p>

<h3 align="center">AI API and MCP gateways</h3>

<p align="center">ThinkWatch Enterprise for organizations, ThinkWatch Lite for individual developers, and ThinkWatch Core, the gateway engine they share.</p>

<p align="center">
  <a href="https://thinkwat.ch/">Website</a> ·
  <a href="https://thinkwat.ch/docs/">Documentation</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch">ThinkWatch Enterprise</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch-Lite">ThinkWatch Lite</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch-Core">ThinkWatch Core</a>
</p>

---

AI clients such as Claude Code, Codex and Cursor call model providers
directly, each with its own API key and without a shared record of what was
sent or what it cost. ThinkWatch places a gateway on that path. Each request
is routed to an upstream, inspected in both directions and recorded with its
cost. ThinkWatch Enterprise adds organization-wide authentication,
permissions, rate limits and budgets, and passes MCP tool calls through the
same gateway.

<table>
  <tr>
    <td width="33%" valign="top">
      <h3>ThinkWatch Enterprise</h3>
      <p><b>For organizations</b></p>
      <p>A self-hosted AI API and MCP gateway with a web console: virtual keys, SSO through OIDC, role-based access control, audit logs, rate limits and budgets.</p>
      <p><sub>Rust · React · PostgreSQL · Redis · ClickHouse<br>Docker Compose or Kubernetes<br>Business Source License 1.1</sub></p>
      <p><a href="https://github.com/ThinkWatchProject/ThinkWatch"><b>ThinkWatchProject/ThinkWatch</b></a></p>
    </td>
    <td width="33%" valign="top">
      <h3>ThinkWatch Lite</h3>
      <p><b>For individual developers</b></p>
      <p>A desktop app for a local AI API gateway. It shows what each request cost, where it was routed and what was redacted, and it can also connect to ThinkWatch Core running on a server.</p>
      <p><sub>Tauri 2 · React 19 · Rust<br>macOS · Windows · Linux<br>MIT License</sub></p>
      <p><a href="https://github.com/ThinkWatchProject/ThinkWatch-Lite"><b>ThinkWatchProject/ThinkWatch-Lite</b></a></p>
    </td>
    <td width="33%" valign="top">
      <h3>ThinkWatch Core</h3>
      <p><b>For servers and for projects built on the engine</b></p>
      <p>Rust crates and the <code>twcore</code> gateway binary. It runs inside ThinkWatch Lite, or on its own as a gateway on a Linux server.</p>
      <p><sub>Rust<br>macOS · Windows · Linux<br>MIT License</sub></p>
      <p><a href="https://github.com/ThinkWatchProject/ThinkWatch-Core"><b>ThinkWatchProject/ThinkWatch-Core</b></a></p>
    </td>
  </tr>
</table>

## Choosing a product

| Requirement | Product |
|---|---|
| Central control of AI access for a team or a company: keys, permissions, audit and budgets | **ThinkWatch Enterprise** |
| Governance of MCP tool calls across an organization | **ThinkWatch Enterprise** |
| A record of what one developer's coding agents cost and where their requests went | **ThinkWatch Lite** |
| A gateway on a Linux server, managed from ThinkWatch Lite on a desktop | **ThinkWatch Core** with **ThinkWatch Lite** |
| The routing, pricing and redaction engine as Rust crates | **ThinkWatch Core** |

---

## ThinkWatch Enterprise

ThinkWatch Enterprise is an AI bastion host. In the way that an SSH bastion
host is the single entry point for access to servers, ThinkWatch Enterprise is
the single entry point for an organization's AI API requests and MCP tool
calls.

```
                    ┌──────────────────────────────────────┐
 Claude Code ──────>│                                      │──> OpenAI
 Cursor ───────────>│    Gateway  :3000                    │──> Anthropic
 Custom Agent ─────>│    AI API + MCP Unified Proxy        │──> Google Gemini
 CI/CD Pipeline ───>│                                      │──> Azure OpenAI / AWS Bedrock
                    └──────────────────────────────────────┘
                    ┌──────────────────────────────────────┐
 Admin Browser ────>│    Console  :3001                    │
                    │    Management UI + Admin API         │
                    └──────────────────────────────────────┘
```

- **AI API gateway.** The OpenAI Chat Completions, Anthropic Messages, OpenAI
  Responses and Gemini APIs on one port, in front of OpenAI, Anthropic, Google
  Gemini, Azure OpenAI, AWS Bedrock and OpenAI-compatible providers. Virtual
  API keys carry their own scopes, rotation and expiry.
- **MCP gateway.** Upstream MCP servers can receive the calling user's own
  OAuth token or access token, stored encrypted, instead of a shared service
  account. Tools are namespaced per server and granted per role and per key,
  and every invocation is recorded in the audit log.
- **Security and compliance.** Single sign-on through any OIDC provider, five
  built-in roles, PII redaction, and AES-256-GCM encryption of provider keys
  and user credentials at rest.
- **Rate limits and budgets.** Sliding-window request and token limits, and
  daily, weekly or monthly token budgets, per user, API key, provider or MCP
  server, with budget alerts.
- **Observability.** Audit logs of every AI request and tool call, stored in
  ClickHouse and optionally forwarded to syslog, Kafka or webhooks; usage and
  cost analytics; Prometheus metrics.

ThinkWatch Enterprise is deployed with Docker Compose or on Kubernetes with a
Helm chart. The [Quick Start](https://github.com/ThinkWatchProject/ThinkWatch#quick-start)
runs it locally, and the [deployment guide](https://thinkwat.ch/docs/deployment-guide/)
covers production.

## ThinkWatch Lite

ThinkWatch Lite is a desktop app that runs a local AI API gateway from the
macOS menu bar, the Windows notification area or the Linux system tray.
Claude Code, Codex and other clients of the Anthropic, OpenAI and Gemini APIs
send their requests to the gateway, and the app records what each request
cost, which upstream served it and what was redacted before it was sent.

- **Cost.** Each request is priced when it completes. The default price sheet
  follows a public price table that is refreshed daily, and custom price
  sheets cover relays that charge differently. Estimated amounts are marked
  as estimates, and usage without a price is reported as unpriced rather than
  counted as zero.
- **Routing.** Rules match on the model, the gateway key, the API format, the
  input size and the features of a request, and send it to an upstream or to
  a group that falls back in order, uses a selected member, takes turns, or
  picks the fastest or the cheapest member. Until the first byte of a
  response arrives, a failing upstream is replaced by the next one. Each
  request records the rule it matched and every upstream attempt, and a dry
  run evaluates the rules for a sample request without sending it.
- **Security.** Five protections, each set to Off, Observe or Enforce:
  redaction of credentials in outgoing requests, restored in the response;
  inspection of the tool calls an upstream returns; detection of hidden
  characters; content rules; and a limit on output length. The first four
  are set to Observe by default and the output limit to Off. Built-in rules
  are listed alongside custom ones, and every match is written to the
  security log.
- **MCP.** The MCP servers configured in each client are shown side by side
  and can be copied between clients or removed; each change is shown before
  it is written, and the original file is backed up. Client configuration,
  skills, hooks, slash commands, subagents and project instructions are
  scanned for hidden characters, prompt injection, dangerous commands and
  overly broad permissions.
- **Clients.** Claude Code, Codex, OpenCode, Zed and Aider are pointed at the
  gateway from the Clients page and can be restored to their previous
  configuration; for Cursor, Continue and Gemini CLI, the page lists the
  manual steps.
- **Menu bar and notifications.** On macOS the menu bar shows today's token
  usage and cost, and its menu lists requests in progress, the output rate
  and the quota of subscription accounts. On Windows and Linux the same menu
  opens from the tray icon. System notifications report conditions that
  need action, such as an exhausted subscription quota, an expired sign-in
  or a blocked tool call.

The interface is available in English and Simplified Chinese. It follows the
system language and can be switched in Settings.

The gateway can also run on a server: ThinkWatch Lite connects to ThinkWatch
Core on another machine over an encrypted control channel, using the key that
`twcore control-key` prints on that machine. Remote connections are added in
Settings › Connection. The app uses one core at a time: while it is connected
to a remote core, the local core stops and its data is kept. The Clients and
MCP pages continue to act on the machine the app runs on.

ThinkWatch Lite is released for macOS 12 or later on Apple silicon, Windows 10
21H2 or later on x64 and ARM64, and Linux on x86_64 and aarch64 (Ubuntu 22.04,
Debian 12, Fedora 36 or later). The gateway ships inside the app, and the app
updates itself; a Homebrew installation is updated through Homebrew.
[thinkwat.ch/lite](https://thinkwat.ch/lite/#install) offers downloads of the
latest version for each platform. On macOS, the app can also be installed
with Homebrew:

```bash
brew install --cask thinkwatchproject/tap/thinkwatch-lite
```

On Linux, one command installs the AppImage:

```bash
curl -fsSL https://github.com/ThinkWatchProject/ThinkWatch-Lite/releases/latest/download/install.sh | sh
```

The disk image, the Windows installers and the AppImages are also attached to
the [latest release](https://github.com/ThinkWatchProject/ThinkWatch-Lite/releases/latest).
The Windows installers are not code-signed: when SmartScreen blocks one,
choose **More info**, then **Run anyway**.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/assets/lite-overview-dark.png">
  <img src="/assets/lite-overview-light.png" alt="The ThinkWatch Lite overview page: tokens, cost and requests for the last seven days compared with the seven days before, a trend chart by model with periods of failures marked, and usage by model">
</picture>

## ThinkWatch Core

ThinkWatch Core is the gateway engine: a set of Rust crates and the `twcore`
binary built from them. It provides rule-based routing, failover between
upstreams before the first byte of a response, cost accounting against a
public price table that is refreshed daily, redaction of credentials in
outgoing requests, and inspection of the tool calls an upstream returns.
ThinkWatch Lite runs `twcore` as its local gateway, and `twcore` also runs on
its own as a gateway on a server. ThinkWatch Enterprise uses three of its
crates: `tw-dialect` for format conversion and usage parsing, `tw-guard` for
redaction and tool-call inspection, and `tw-breaker` for circuit breaking.

Each [release](https://github.com/ThinkWatchProject/ThinkWatch-Core/releases/latest)
includes a `twcore` binary for macOS on Apple silicon, Windows on x64 and
ARM64, and Linux on x86_64 and aarch64, each with a SHA-256 checksum; the
Linux archives also contain the systemd unit.

On a Linux server, one command installs the binary, a dedicated service user
and the systemd unit:

```bash
curl -fsSL https://raw.githubusercontent.com/ThinkWatchProject/ThinkWatch-Core/main/scripts/install.sh | sudo sh
```

[Running core on a server](https://github.com/ThinkWatchProject/ThinkWatch-Core/blob/main/docs/server.md)
describes the configuration, the remote control port that ThinkWatch Lite
connects to, and upgrades with `twcore upgrade`. Every field of `config.yaml`
is described in the [configuration reference](https://github.com/ThinkWatchProject/ThinkWatch-Core/blob/main/docs/config.md).

To build `twcore` from source with Cargo:

```bash
cargo install --git https://github.com/ThinkWatchProject/ThinkWatch-Core --locked twcore
```

This builds the current `main` branch; `--tag v<version>` builds a release
instead.

---

## Repositories

| Repository | Contents | License |
|---|---|---|
| [**ThinkWatch**](https://github.com/ThinkWatchProject/ThinkWatch) | ThinkWatch Enterprise: the gateway server, the MCP gateway and the web console | BSL 1.1 |
| [**ThinkWatch-Lite**](https://github.com/ThinkWatchProject/ThinkWatch-Lite) | ThinkWatch Lite, the desktop app for macOS, Windows and Linux | MIT |
| [**ThinkWatch-Core**](https://github.com/ThinkWatchProject/ThinkWatch-Core) | ThinkWatch Core: the Rust crates and the `twcore` binary | MIT |
| [**homebrew-tap**](https://github.com/ThinkWatchProject/homebrew-tap) | The Homebrew cask for ThinkWatch Lite | MIT |
| [**thinkwatch.github.io**](https://github.com/ThinkWatchProject/thinkwatch.github.io) | Source of [thinkwat.ch](https://thinkwat.ch/) | |

## Licenses

**ThinkWatch Lite** and **ThinkWatch Core** are released under the MIT License.

**ThinkWatch Enterprise** is source-available under the [Business Source License 1.1](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSE).
Non-production use is free. Production use is free up to `10,000,000` Billable
Tokens and `10,000` MCP Tool Calls per UTC calendar month; above either
threshold, a commercial license is required.
[LICENSING.md](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSING.md)
sets out the full terms, the tiering model and the conversion of each version
to GPL-2.0-or-later, and [thinkwat.ch/license](https://thinkwat.ch/license/)
gives the contact for commercial licenses.

## Security

Vulnerabilities are reported privately, as described in the
[security policy](https://github.com/ThinkWatchProject/.github/blob/main/SECURITY.md).
