<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="/assets/logo-dark.png">
    <img src="/assets/logo.png" alt="ThinkWatch" width="480">
  </picture>
</p>

<h3 align="center">The Control Plane for Autonomous Agents</h3>

<p align="center">
  <b>Enterprise-grade gateway for AI — Secure, audit, and govern every AI API call and MCP tool invocation across your organization.</b>
</p>

<p align="center">
  <a href="https://github.com/ThinkWatchProject/ThinkWatch/stargazers">
    <img src="https://img.shields.io/github/stars/ThinkWatchProject/ThinkWatch?style=social" alt="GitHub Stars" />
  </a>
  &nbsp;
  <img src="https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white" />
  <img src="https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB" />
  <img src="https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white" />
</p>

---

## 🏰 What is ThinkWatch?

Just as an **SSH bastion host** is the single gateway through which all server access must flow, **ThinkWatch** is the single gateway through which all AI access must flow.

> Every model request. Every tool call. Every token. **Authenticated, authorized, rate-limited, logged, and accounted for.**

As AI agents proliferate across engineering teams, organizations face a growing governance challenge — API keys scattered everywhere, zero visibility into usage, no access control, compliance gaps, and cost surprises. **ThinkWatch solves all of this with a single deployment.**

```
                    ┌──────────────────────────────────────┐
 Claude Code ──────>│                                      │──> OpenAI
 Cursor ───────────>│    Gateway  :3000                    │──> Anthropic
 Custom Agent ─────>│    AI API + MCP Unified Proxy        │──> Google Gemini
 CI/CD Pipeline ───>│                                      │──> Azure OpenAI / AWS Bedrock
                    └──────────────────────────────────────┘
                    ┌──────────────────────────────────────┐
 Admin Browser ────>│    Console  :3001                    │
                    │    Management UI + Admin API          │
                    └──────────────────────────────────────┘
```

---

## ✨ Core Features

<table>
  <tr>
    <td>🔑 <b>Virtual API Keys</b></td>
    <td>Issue scoped <code>tw-</code> keys per team, project, or developer. Automatic rotation with grace periods, inactivity timeout, and expiry warnings.</td>
  </tr>
  <tr>
    <td>🔀 <b>Multi-Provider Routing</b></td>
    <td>OpenAI, Anthropic, Google Gemini, Azure OpenAI, AWS Bedrock — all behind a single unified endpoint. Drop-in replacement for Cursor, Cline, Claude Code, and OpenAI/Anthropic SDKs.</td>
  </tr>
  <tr>
    <td>🛠️ <b>MCP Gateway</b></td>
    <td>Centralized tool proxy with namespace isolation (<code>github__create_issue</code>, <code>postgres__query</code>), tool-level RBAC, and full audit trail for every invocation.</td>
  </tr>
  <tr>
    <td>💰 <b>Cost Tracking</b></td>
    <td>Per-model pricing with budget alerts, team attribution, and month-to-date spend analytics. No more unexplained AI bills.</td>
  </tr>
  <tr>
    <td>🔒 <b>RBAC & SSO</b></td>
    <td>5-tier role-based access control (Super Admin → Viewer). Plug into Zitadel, Okta, Azure AD, or any OIDC provider.</td>
  </tr>
  <tr>
    <td>📋 <b>Audit Logs</b></td>
    <td>Full-text searchable audit trail powered by Quickwit with S3-backed cloud-native storage. Forward to any SIEM via Syslog, Kafka, or HTTP webhook.</td>
  </tr>
  <tr>
    <td>⚡ <b>Rate Limiting</b></td>
    <td>Sliding-window RPM/TPM limits via Redis, per key or per user. Built-in circuit breaker with configurable threshold and retry backoff.</td>
  </tr>
  <tr>
    <td>📈 <b>Prometheus Metrics</b></td>
    <td>Ready-to-use <code>/metrics</code> endpoint with request counts, latency histograms, token totals, rate limit stats, and circuit breaker state.</td>
  </tr>
  <tr>
    <td>🛡️ <b>Security-First Design</b></td>
    <td>AES-256-GCM encryption at rest, distroless containers (2 MB runtime), dual-port architecture, CSP headers, and SHA-256 key hashing.</td>
  </tr>
  <tr>
    <td>🔧 <b>Dynamic Configuration</b></td>
    <td>Web UI settings console, first-run setup wizard, built-in configuration guide for popular AI clients, and multi-instance sync via Redis Pub/Sub.</td>
  </tr>
</table>

---

## 🚀 Quick Start

```bash
# 1. Start infrastructure (PostgreSQL, Redis, Quickwit, Zitadel)
docker compose -f deploy/docker-compose.dev.yml up -d

# 2. Configure and start the backend (Gateway :3000 + Console :3001)
cp .env.example .env
cargo run -p think-watch-server

# 3. Start the frontend dev server
cd web && pnpm install && pnpm dev

# 4. Complete the setup wizard at http://localhost:5173/setup
```

---

## 🏗️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Rust · Axum 0.8 · SQLx 0.8 · OpenTelemetry |
| **Frontend** | React 19 · TypeScript 6 · Vite 8 · shadcn/ui · Tailwind CSS 4 |
| **Database** | PostgreSQL 18 |
| **Cache & Rate Limiting** | Redis 8 |
| **Audit Log Search** | Quickwit 0.8 (S3-backed, cloud-native) |
| **Object Storage** | AWS S3 / GCS / Azure Blob / RustFS (S3-compatible) |
| **SSO** | Zitadel (or any OIDC provider) |
| **Containers** | Distroless · Helm Chart for Kubernetes |

---

## 📦 Port Architecture

| Port | Server | Exposure | Purpose |
|------|--------|----------|---------|
| `3000` | Gateway | **Public** — expose to AI clients | `/v1/chat/completions`, `/v1/messages`, `/v1/responses`, `/mcp`, `/metrics`, `/health/*` |
| `3001` | Console | **Internal** — behind VPN/firewall | `/api/*` management endpoints · Web UI |

> In production, **only port 3000** should be reachable from the internet. Port 3001 should be restricted to your admin network.

---

## 📚 Repositories

| Repository | Description |
|-----------|-------------|
| [**ThinkWatchProject/ThinkWatch**](https://github.com/ThinkWatchProject/ThinkWatch) | 🛡️ Core platform — AI gateway server, proxy, MCP proxy, and web console (Rust + React) |

---

## 📄 License

ThinkWatch is source-available under the [Business Source License 1.1](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSE).  
Non-production use is **free**. Production use is **free** up to `10,000,000` Billable Tokens and `10,000` MCP Tool Calls per UTC calendar month.

See [LICENSING.md](https://github.com/ThinkWatchProject/ThinkWatch/blob/main/LICENSING.md) for full details, tiering model, and the changeover to GPL-2.0-or-later.

---

<p align="center">Made with ❤️ for AI-native engineering teams</p>
