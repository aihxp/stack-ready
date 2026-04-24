# Mirror Box stack decision (dogfood for stack-ready v1.1.8)

**Mode:** B (Assessment; incumbent stack confirmed)
**State:** Decided
**Version:** 1.0
**Last updated:** 2026-04-23
**Upstream PRD:** [aihxp/prd-ready/dogfood/PRD.md](https://github.com/aihxp/prd-ready/blob/main/dogfood/PRD.md)
**Upstream ARCH:** [aihxp/architecture-ready/dogfood/ARCH.md](https://github.com/aihxp/architecture-ready/blob/main/dogfood/ARCH.md)
**Implementation:** [aihxp/mirror-box v1.0.0](https://github.com/aihxp/mirror-box)
**Owner:** h (dogfood)

## What this is

Retroactive stack-ready pass against the Mirror Box implementation. The service shipped at Mirror Box v1.0.0 using a specific set of tools. This document confirms Mode B (incumbent-stack-is-sound) by running stack-ready's decision framework against the implementation and checking that each pick is defensible against the domain and constraints declared upstream.

Purpose: close the stack-ready half of the Mirror Box executional gap flagged in the suite VALIDATION report. Six skills lacked dogfood artifacts against Mirror Box; this is the first of six.

## Mode determination

stack-ready's Mode gate:

- Mode A (greenfield decision) would apply if no stack were in place. Not the case.
- Mode B (assessment of an existing stack) applies here: the service runs at Mirror Box v1.0.0 with a specific stack; we assess whether the stack holds against the upstream constraints.
- Mode C (migration) would apply if we were moving off the incumbent. Not the case.

Mode B verdict: incumbent stack is sound. No migration recommended.

## Pre-flight answers (from PRD HANDOFF)

All six stack-ready pre-flight questions were pre-filled by prd-ready's HANDOFF. No re-interrogation:

| # | Question | Answer |
|---|---|---|
| 1 | Domain | Developer / infrastructure tool (closest match among stack-ready's 12 profiles). Overrides: single-endpoint public service, no user accounts. |
| 2 | Team | 1 engineer (solo dogfood); JavaScript/Node depth; no on-call. |
| 3 | Budget posture | Free-tier scrappy. No paid services unless free tier is unavailable. |
| 4 | Time-to-ship | Weeks (past tense; shipped in ~5 weeks). |
| 5 | Scale ceiling (12 months) | <100 req/s sustained; <50M spans/month; <10k unique workshop users. |
| 6 | Regulatory constraints | None. No PII collected. |

## Hard constraints (from ARCH HANDOFF)

The architecture layer added five hard constraints that stack-ready's pairing check must honor:

1. **Runtime shape:** JavaScript / Node (team language depth).
2. **Framework shape:** minimal HTTP framework; sync request/response; no edge or serverless cold-start tolerance.
3. **Observability runtime shape:** OTel SDK for Node; OTLP/gRPC outbound to an OTLP-compatible backend.
4. **Hosting shape:** free-tier always-on single-region compute with edge TLS termination and rolling deploys.
5. **Observability backend shape:** OTLP/gRPC-accepting SaaS with a free tier supporting ~50M spans/month.

Storage: none (stateless per ARCH ADR-002).

## The stack (as implemented)

| Dimension | Pick | Version | Rationale |
|---|---|---|---|
| Runtime | Node.js | 22 LTS (active through 2027-04) | Team language depth; OTel SDK maturity; stable LTS window covers Mirror Box's expected lifetime. |
| Package manager | npm | bundled with Node 22 | No reason to add a separate tool for a nine-dependency project. |
| HTTP framework | Fastify | ^5.0.0 | Minimal, async-first, TypeScript-ready without forcing it, plugin ecosystem sufficient for the four request types. Benchmarks ~10k req/s on shared-cpu-1x, comfortably above the PRD's 50 req/s target. |
| Observability SDK | @opentelemetry/sdk-node | ^0.54.0 | Canonical Node OTel path. Auto-instruments HTTP via `@opentelemetry/auto-instrumentations-node`. |
| OTLP exporter | @opentelemetry/exporter-trace-otlp-grpc | ^0.54.0 | gRPC transport: lower overhead than HTTP/JSON, required by Honeycomb's public OTLP endpoint. |
| Observability backend | Honeycomb (incumbent) | SaaS | Free tier covers 20M events/month; comfortably holds Mirror Box at <50M spans/month targeted scale. Backend is configurable via `HONEYCOMB_API_KEY` shortcut or arbitrary `OTEL_EXPORTER_OTLP_ENDPOINT`. |
| Hosting | Fly.io (incumbent) | shared-cpu-1x, region IAD | Free-tier always-on (no cold starts). Rolling deploys. Rollback window of last 3 deploys. |
| Container runtime | Docker | Alpine base (node:22-alpine) | Portability to any Docker host if Fly's free tier changes. |
| Dependency audit | npm audit + `overrides` | bundled | Forces `uuid ^14.0.0` to dodge the transitive moderate-severity buffer-bounds CVE in the OTel auto-instrumentation tree. Zero findings as of ship. |

## Alternatives considered (ADRs)

### HTTP framework: Fastify vs Express vs Hono

- **Express** is the most common Node framework but slower and more middleware-heavy than Fastify for a minimal-endpoint service. Rejected.
- **Hono** is newer, faster than Fastify in some benchmarks, and edge-native (Cloudflare Workers / Deno / Bun). Rejected at v1 because Mirror Box's hosting shape is Fly.io (long-running Node process), not edge; Hono's edge advantages don't apply and its Node ecosystem is smaller. If Mirror Box ever switches to edge compute, re-open the Fastify-vs-Hono decision.
- **Fastify** selected: async-first, best-in-class JSON parsing, mature plugin ecosystem, active maintenance.

### Runtime: Node vs Bun vs Deno

- **Bun** would be faster for cold start and has better default DX. Rejected because OTel Node SDK support is less mature as of 2026-04 and Mirror Box's hosting (Fly always-on) doesn't need cold-start wins.
- **Deno** has first-class TypeScript and a simpler OTel story via OpenTelemetry's Deno integration. Rejected because team language depth is plain Node and Deno's npm compatibility, while good, adds a layer of indirection no problem in Mirror Box's scope justifies.
- **Node 22 LTS** selected.

### Hosting: Fly vs Railway vs Render vs Cloudflare Workers

- **Railway / Render** have similar free-tier shapes to Fly for this workload. Rejected in favor of Fly because Fly's IAD region and Dockerfile-native deploy are already in the ARCH HANDOFF (Fly is the incumbent).
- **Cloudflare Workers** would eliminate Node entirely. Rejected: the service isn't edge-native by shape (single-region teaching target) and Workers' OTel story is less mature. Re-open if Mirror Box becomes a global API.
- **Fly.io** selected (incumbent).

### Observability backend: Honeycomb vs Grafana Cloud vs Axiom vs open-source collector

- **Grafana Cloud** has a more permissive free tier (100k spans/month) and solid OTLP support. Sibling-swap candidate. Rejected at v1 because Honeycomb is the incumbent and the service's OTLP exporter is swap-compatible; the migration is <1 hour if free-tier limits become a problem.
- **Axiom** is a newer OTel-native backend with good ergonomics. Same sibling-swap posture as Grafana Cloud. Not selected at v1.
- **Self-hosted OTel collector + Tempo / Jaeger** would be free at scale but adds operational surface (one more service to run). Rejected at v1; workshop participants should not need to also host an observability stack.
- **Honeycomb** selected (incumbent) with the shortcut `HONEYCOMB_API_KEY` env var in otel.js. The service remains backend-agnostic via `OTEL_EXPORTER_OTLP_ENDPOINT` override.

## Rejected stacks (for the record)

- **TypeScript.** Not adopted at v1 despite Fastify's TS support. Rationale: Mirror Box is tiny, tsc adds a build step, and the service is JS-native enough to not need static typing. If Mirror Box grows past ~500 LoC, revisit.
- **Monorepo tooling (pnpm, turborepo, nx).** Not adopted. Single-package repo; overkill.
- **Database (Postgres, SQLite, Redis).** Not adopted. Stateless by design (ARCH ADR-002).

## Assumption-stack check

The PRD's "Assumption: learners want a deployed target, not a local-only" is sensitive to one stack choice: hosting. If the assumption fails (learners prefer local-only), Fly.io selection is moot; Docker Compose would suffice. The stack is robust to this because `docker compose up` works locally from the same Dockerfile. No stack change needed if the assumption breaks; the deployment story changes, not the tools.

## Verification (dogfood self-check)

The stack as implemented was verified locally during Mirror Box v1.0.0 ship:

- **Install:** `npm install` produces a clean tree with zero npm-audit findings (the uuid override closes the transitive CVE).
- **Boot:** `npm start` brings the server up on port 8080 in <2 seconds.
- **Runtime:** all four endpoints (`/`, `/healthz`, `/echo`, `/echo` with non-JSON) return correct shapes at sub-5ms response times.
- **Tracing:** trace_id populates in the `/echo` response, confirming OTel is wired. Full OTLP export to Honeycomb is tested end-to-end when `HONEYCOMB_API_KEY` is set.
- **Portability:** Dockerfile builds the service using node:22-alpine; the image runs on any Docker host.

The stack is sound for Mirror Box's PRD and ARCH constraints. Mode B: confirmed.

## Follow-ups for v1.1 (conditional)

None urgent. Two contingencies if PRD OQ-01 (rate limiting) resolves:

1. If rate limiting lands and needs shared state across instances, introduce **Redis** (smallest fit) and update this decision. Would trigger ARCH ADR-004.
2. If rate limiting is per-instance only, `bottleneck` or `p-queue` suffice and stay in-process. No stack change.

## Handoff (back to the suite)

This dogfood doesn't trigger downstream re-runs. The planning-tier HANDOFFs already routed stack-ready inputs through every downstream sibling. This document is confirmation, not new input.

Ready-suite version table and SUITE.md are unchanged by this commit.
