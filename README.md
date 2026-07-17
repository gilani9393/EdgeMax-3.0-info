# EdgeMax-3.0-info
EdgeMax - Trading Oprating System
## Built with Codex & GPT-5.6

EdgeMax 3.0's entire Rust codebase was built through AI-assisted development using
Codex (GPT-5.6 Extra High) as the primary implementation agent across extended
autonomous sessions.

**How it was used:**
- I (the sole architect) explain ICT/Smart Money Concepts trading logic — market
  structure shifts, liquidity sweeps, fair value gaps, order flow, multi-timeframe
  context — in detailed natural-language specifications.
- Codex implements this logic as working Rust code, including domain modules,
  test suites, and remediation fixes across the codebase.
- Example: Codex diagnosed and fixed a server-side throughput bottleneck (a
  synchronous fsync call inside a global mutex) that was capping the system at
  ~185 records/second, restoring confirmed superlinear concurrency scaling.
- Example: a 7-agent parallel Codex session resolved a full audit remediation
  plan — 28 new value-asserting tests, cross-module verification across 10
  Market Intelligence modules (Market Structure, Liquidity, PD Arrays, Asset
  Sync, Time Profile, Volume, Directional Bias, Historical Patterns, Analysis
  Continuity, NEXUS).
- Every Codex-reported "done" or "approved" claim is independently re-verified
  against the actual code before being trusted — this caught a real production
  bug (a PD Arrays detection function left structurally unreachable) that test
  logs alone did not surface.


EdgeMax 3.0 — Trading Operating System
Core mission: Centralized platform for collecting market data, executing broker-side commands, and analyzing trading infrastructure across multiple terminals (MT5, cTrader) and data sources.

Key operational components:
Edge Collector (Feeder) — Windows service that polls MT5, collects technical/fundamental data, and spools it durably for uplink
Central Backend (Server) — Rust HTTP API that journals all ingress, persists to PostgreSQL, runs market analysis workers, and serves query surfaces
Terminal Connectors — MT5 Expert Advisor + cTrader cBot that poll for signed commands and report account facts back to the server
Web Dashboard — React SPA with role-gated workspaces for Signal Desk operators, admins, and partners
Reliability guarantees:
Durability: All work is journaled (JSONL append-only) before mutation
Idempotency: Replay-safe commands with HMAC signing and deduplication
Recovery: Server restarts reconcile pending commands; Feeder has transactional outbox
Deployment:
Linux server (Rust + PostgreSQL) with production scripts for full provisioning, TLS termination via nginx, and Feeder uplink via Cloudflare Tunnel.

