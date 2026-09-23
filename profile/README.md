<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/logo-dark.png">
    <img src="/assets/logo.png" alt="ThinkWatch" width="480">
  </picture>
</p>

<h3 align="center">One gateway between your AI clients and the models they call</h3>

<p align="center">For a whole organization, or for one developer's machine.</p>

<p align="center">
  <a href="https://thinkwat.ch">Website</a> ·
  <a href="https://thinkwat.ch/docs">Docs</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch">ThinkWatch Enterprise</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch-Lite">ThinkWatch Lite</a> ·
  <a href="https://github.com/ThinkWatchProject/ThinkWatch-Core">ThinkWatch Core</a>
</p>

---

Claude Code, Codex, Cursor, and your own agents talk to model providers
directly. API keys end up scattered, nobody sees what was sent, and the bill is
a surprise. ThinkWatch puts a gateway in that path, so every model request and
tool call is routed, checked, and accounted for.

It comes as two products, built for different people.

<table>
  <tr>
    <td width="50%" valign="top">
      <h3>🏢 ThinkWatch Enterprise</h3>
      <p><b>For teams and enterprises</b></p>
      <p>A self-hosted AI API and MCP gateway. Virtual keys, SSO and RBAC, audit logs, rate limits and budgets, and cost attribution, all in one control plane.</p>
      <p>
        <a href="https://github.com/ThinkWatchProject/ThinkWatch/stargazers"><img src="https://img.shields.io/github/stars/ThinkWatchProject/ThinkWatch?style=social" alt="GitHub Stars" /></a>
      </p>
      <p><sub>Rust · React · PostgreSQL · Redis · Kubernetes<br>Business Source License 1.1</sub></p>
      <p><a href="https://github.com/ThinkWatchProject/ThinkWatch"><b>ThinkWatchProject/ThinkWatch</b></a></p>
    </td>
    <td width="50%" valign="top">
      <h3>💻 ThinkWatch Lite</h3>
      <p><b>For individual developers</b></p>
      <p>A desktop app for a local AI API gateway, in the macOS menu bar or the Windows notification area. Point Claude Code, Codex CLI or another client at a local port, and see what each request cost, which upstream served it and why, and what was sent along with it.</p>
      <p><sub>Tauri 2 · React 19 · Rust<br>MIT License · macOS 12 or later, Apple Silicon · Windows 10 or later, x64 or ARM64</sub></p>
      <p><a href="https://github.com/ThinkWatchProject/ThinkWatch-Lite"><b>ThinkWatchProject/ThinkWatch-Lite</b></a></p>
    </td>
  </tr>
</table>

Underneath both sits [**ThinkWatch Core**](https://github.com/ThinkWatchProject/ThinkWatch-Core),
a set of MIT-licensed Rust crates for routing, failover, cost accounting, and
redaction.

## Which one is for you?

| If you… | Use |
|---|---|
| run AI access for a team and need keys, permissions, audit, and budgets in one place | **ThinkWatch Enterprise** |
| need to govern MCP tool calls across an organization | **ThinkWatch Enterprise** |
| are one developer who wants to know what your coding agents cost and where their requests go | **ThinkWatch Lite** |
| are building your own gateway, or want the routing and redaction engine as a library | **ThinkWatch Core** |

---

## 🏢 ThinkWatch Enterprise: the AI bastion host

Just as an SSH bastion host is the single gateway through which all server
access flows, ThinkWatch Enterprise is the single gateway through which all AI
access flows.

> Every model request. Every tool call. Every token. **Authenticated, authorized, rate-limited, logged, and accounted for.**

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

- **AI API gateway.** OpenAI, Anthropic, Google Gemini, Azure OpenAI, and AWS Bedrock behind one endpoint, with scoped virtual keys per team, project, or developer.
- **MCP gateway.** A central tool proxy with namespace isolation, tool-level RBAC, and an audit trail for every invocation.
- **Security and compliance.** SSO through any OIDC provider, role-based access control, PII redaction, and encryption at rest.
- **Rate limits and budgets.** Request and token limits per key or per user, with spend budgets and alerts.
- **Observability.** Searchable audit logs, cost analytics, and Prometheus metrics.

Start with the [Quick Start](https://github.com/ThinkWatchProject/ThinkWatch#quick-start), or read the [docs](https://thinkwat.ch/docs).

## 💻 ThinkWatch Lite: your local gateway, on your desktop

- **What it cost, and how far to trust that number.** Measured, estimated, and unpriced are reported separately and never added together; usage served by a subscription is counted apart from billed usage.
- **Where each request went, and why.** The rule it matched, the group it went through, and every upstream attempt with its status and duration. A dry run answers the same question before any traffic.
- **What went out with it.** Secrets replaced on their way to an untrusted upstream and restored in the response, dangerous tool calls cut off mid-stream, and client configuration files scanned for hidden characters and dangerous commands.
- **Cost at a glance.** Today's cost and output rate, or the quota left on a subscription account, right in the menu bar or the notification area.

Released for macOS 12 or later on Apple Silicon, and for Windows 10 or later on
x64 and ARM64. The gateway ships inside the app, and the app updates itself. On
macOS:

```bash
brew install --cask thinkwatchproject/tap/thinkwatch-lite
```

Or download the disk image from the
[releases page](https://github.com/ThinkWatchProject/ThinkWatch-Lite/releases),
which also has the Windows installers. The Windows installers are not
code-signed: when SmartScreen stops one, choose **More info**, then **Run anyway**.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/assets/lite-overview-dark.png">
  <img src="/assets/lite-overview-light.png" alt="ThinkWatch Lite's usage overview: tokens, cost and requests, a 24-hour trend stacked by model, the leaderboard by model and the cache hit rate">
</picture>

## 🧩 ThinkWatch Core: the shared engine

Rule-based routing, mid-flight failover, cost accounting against a price
snapshot, outbound secret redaction, and inspection of tool calls coming back
from upstream. `twcore` is a complete, self-contained gateway binary built from
these crates:

```bash
cargo run -p twcore -- init     # write a commented config.yaml
cargo run -p twcore -- serve    # start the gateway and control plane
```

---

## 📚 Repositories

| Repository | What it is | License |
|---|---|---|
| [**ThinkWatch Enterprise**](https://github.com/ThinkWatchProject/ThinkWatch) | Self-hosted AI API and MCP gateway: server, proxy, and web console | BSL 1.1 |
| [**ThinkWatch Lite**](https://github.com/ThinkWatchProject/ThinkWatch-Lite) | macOS menu-bar app for individual developers | MIT |
| [**ThinkWatch Core**](https://github.com/ThinkWatchProject/ThinkWatch-Core) | Shared gateway engine, as Rust crates and the `twcore` binary | MIT |
| [**thinkwatch.github.io**](https://github.com/ThinkWatchProject/thinkwatch.github.io) | Source of [thinkwat.ch](https://thinkwat.ch) | |

## 📄 License

**ThinkWatch Lite** and **ThinkWatch Core** are released under the MIT License.

**ThinkWatch Enterprise** is source-available under the [Business Source License 1.1](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSE).
Non-production use is free. Production use is free up to `10,000,000` Billable
Tokens and `10,000` MCP Tool Calls per UTC calendar month. See
[LICENSING.md](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSING.md)
for the full terms, the tiering model, and the changeover to GPL-2.0-or-later.

---

<p align="center">Made with ❤️ for AI-native engineers and teams</p>
