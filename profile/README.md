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
  <a href="https://github.com/ThinkWatchProject/ThinkWatch">ThinkWatch</a> ·
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
      <h3>🏢 ThinkWatch</h3>
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
      <p>A desktop app for a local AI API gateway. Point Claude Code or Codex at a local port, and see what a session cost, where each request was routed, and what was redacted before it left your machine.</p>
      <p><sub>Tauri 2 · React 19 · Rust<br>MIT License · in development, macOS first</sub></p>
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
| run AI access for a team and need keys, permissions, audit, and budgets in one place | **ThinkWatch** |
| need to govern MCP tool calls across an organization | **ThinkWatch** |
| are one developer who wants to know what your coding agents cost and where their requests go | **ThinkWatch Lite** |
| are building your own gateway, or want the routing and redaction engine as a library | **ThinkWatch Core** |

---

## 🏢 ThinkWatch: the AI bastion host

Just as an SSH bastion host is the single gateway through which all server
access flows, ThinkWatch is the single gateway through which all AI access
flows.

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

## 💻 ThinkWatch Lite: your local gateway, in the menu bar

- **What a session cost, and how far to trust that number.** Measured, estimated, and unpriced are shown separately, never added together.
- **Where each request went, and why.** The rule it matched, the policy group, and the full failover chain.
- **What went out with it.** Secrets caught on their way to an untrusted upstream, redactions applied, and tool calls that looked dangerous.
- **Spend at a glance.** Today's spend, or remaining subscription quota, right in the menu bar.

ThinkWatch Lite is in development. macOS comes first, and for now you run it
from source:

```bash
pnpm install
pnpm tauri dev
```

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
| [**ThinkWatch**](https://github.com/ThinkWatchProject/ThinkWatch) | Enterprise AI API and MCP gateway: server, proxy, and web console | BSL 1.1 |
| [**ThinkWatch Lite**](https://github.com/ThinkWatchProject/ThinkWatch-Lite) | Desktop app for individual developers | MIT |
| [**ThinkWatch Core**](https://github.com/ThinkWatchProject/ThinkWatch-Core) | Shared gateway engine, as Rust crates and the `twcore` binary | MIT |
| [**thinkwatch.github.io**](https://github.com/ThinkWatchProject/thinkwatch.github.io) | Source of [thinkwat.ch](https://thinkwat.ch) | |

## 📄 License

**ThinkWatch Lite** and **ThinkWatch Core** are released under the MIT License.

**ThinkWatch** is source-available under the [Business Source License 1.1](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSE).
Non-production use is free. Production use is free up to `10,000,000` Billable
Tokens and `10,000` MCP Tool Calls per UTC calendar month. See
[LICENSING.md](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSING.md)
for the full terms, the tiering model, and the changeover to GPL-2.0-or-later.

---

<p align="center">Made with ❤️ for AI-native engineers and teams</p>
