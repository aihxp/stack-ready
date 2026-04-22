# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.0] - 2026-04-22

Research-driven refresh. Six parallel research threads (framework, data, auth/payments/email, hosting/observability/queues, decision methodology, cross-language) produced a consolidated dossier and surgical updates across every reference file. No architectural changes; all edits are evidence-backed against primary sources captured in `references/RESEARCH-2026-04.md`.

### Added

- **`references/RESEARCH-2026-04.md`** - the single source-of-truth dossier for 1.1.0's refresh. ~30K tokens. ~150 verified URLs across Stack Overflow, State of JS, Rising Stars, DB-Engines, ThoughtWorks Radar, vendor release notes, and migration postmortems. Structured per-dimension findings, meta-findings on scoring methodology, compliance snapshot, cross-dimension migration signal table.
- **Scoring framework: split `Ecosystem / maturity (15%)` into two axes** - `Ecosystem depth (8%)` and `Ecosystem trajectory (7%)`. Motivated by State of JS's distinct retention vs interest axes; Prisma/Drizzle is the canonical test case (depth vs trajectory diverge).
- **Scoring framework: new `Exit cost / reversibility (7%)` dimension**, redistributed from Ops and Cost. Convex/Firebase high DX but higher exit cost; Postgres + thin ORM scores high on exit cost. Used to be tradeoff-narrative-only; now a first-class score.
- **Scoring framework: "When to skip the scoring pass entirely" section** acknowledging the McKinley/DHH/Levels anti-scoring camp. Experience-backed, reversible, low-stakes decisions by small teams should run compact pre-flight only; formal scoring applied where it's cost not value is itself a failure mode.
- **Pre-flight: question 7 (Reversibility posture)** - Exploratory / Committed / Foundational. Includes per-dimension reversibility defaults (DB foundational, ORM committed, email exploratory, etc.).
- **SKILL.md: "team already knows" exit valve** in "When this skill does NOT apply." Routes stated-preference + reversible + no-constraint-change cases to compact-pre-flight-only.
- **SKILL.md DECISION.md template: `Prior art and analogous picks` section** (Rust RFC influence). Three real deployments with comparable stack at comparable scale; the strongest defense against horoscope-shaped recommendations.
- **5 new cross-domain bundle archetypes in `stack-bundles.md`**: Cloudflare-native (Workers + D1 + R2 + Durable Objects), Post-Cloud Rails (Rails 8 + Kamal 2 + Hetzner + Solid Queue), Phoenix LiveView, Django + HTMX + Alpine, TALL / Laravel + Livewire. Non-React alternatives promoted from Fast-to-Ship-only to first-class patterns.
- **Pairing rules: sync-engine anti-pairing** (Convex + Liveblocks over same state, Supabase Realtime + Convex).
- **Pairing rules: Auth.js + Better Auth anti-pairing** for new projects (Auth.js now in security-patch mode post September 2025 merge).
- **Pairing rules: extensive staleness refresh** covering Auth.js/Better Auth merger (Sept 2025), Remix v2 to React Router v7 (Nov 2024), Stripe + Lemon Squeezy acquisition (July 2024), Stripe Managed Payments (April 2025), Prisma 7 Rust-engine removal (late 2025), Neon + Databricks (May 2025), PlanetScale Postgres on Neki (Sept 2025), Redis fragmentation (2024, Valkey/DragonflyDB/Upstash), Highlight.io EOL (Feb 2026), Bun + Anthropic (Dec 2025), Vercel Fluid Compute (April 2025), Cloudflare Dev Platform GA.
- **Dimension deep-dives: LLM observability subsection** (Braintrust, Langfuse, Helicone, LangSmith, Phoenix).
- **Dimension deep-dives: TypeScript backend subsection** (Hono as edge/multi-runtime leader, Fastify Node-native, Elysia Bun-specific, NestJS enterprise, Express declining, tRPC as typed RPC layer).
- **Dimension deep-dives: JS runtime sub-axis** (Node 24 LTS / Node 22 LTS / Bun / Deno 2 production posture).
- **Dimension deep-dives: session replay as a separate lane** (Sentry Replay default, OpenReplay OSS, Highlight.io EOL).

### Changed

- **Default weight vector** rebalanced: DX 20 / Scale 13 / Cost 13 / EcoDepth 8 / EcoTraj 7 / Exit 7 / Ops 12 / Compliance 10 / Hireability 10 (sums to 100). Replaces the 1.0.0 7-dimension weight.
- **`Remix v2+`** references updated to `React Router v7 (formerly Remix v2)` across domain-stacks.md (merger was November 2024; Remix v3 forks Preact and is a separate product).
- **`Auth.js (7)`** references downgraded to `Auth.js (5, now in security-patch mode; for new projects use Better Auth)` across domain-stacks.md. Reflects the September 2025 Auth.js folding into Better Auth.
- **`Lemon Squeezy (7)`** references updated to `Polar (8, low-fee MoR) / Lemon Squeezy (6, acquired by Stripe, absorbing into SMP)` across domain-stacks.md.
- **Prisma scoring rationale updated** in dimension-deep-dives.md: historical penalties for bundle size and serverless cold start are stale as of Prisma 7 (late 2025). Prisma remains behind Drizzle on SQL transparency / bundle floor; parity on edge / cold start.
- **SendGrid scoring dropped from 7 to 6** in dimension-deep-dives.md: free tier removed in 2025; new accounts get 60-day trial then $19.95/mo minimum.
- **Grafana Cloud scoring bumped from 7 to 8** in dimension-deep-dives.md: primary migration target from Datadog with ~40% typical savings (50+ migrations per Grafana Labs).
- **Cloudflare scoring bumped from 7 to 8-9** in dimension-deep-dives.md: D1/Queues/Hyperdrive/Workflows GA since April 2024; Durable Objects with SQLite on free tier; Containers launched mid-2025.
- **Vercel scoring nuanced** in dimension-deep-dives.md: still 8 general, 9 for AI-agent apps post Fluid Compute (April 2025), 7 for content-heavy or high-egress workloads.
- **Frontmatter: version bumped to 1.1.0.** Session state and decision artifact templates updated to reference 1.1.0.
- **Reference library manifest extended** with `RESEARCH-2026-04.md`.

### Refreshed evidence base

All 1.1.0 scoring shifts are traceable to primary sources captured in `references/RESEARCH-2026-04.md`. Notable evidence underpinning the scoring shifts:

- Stack Overflow Developer Survey 2024 (Postgres #1 admired; Rust 72% admired for 10 consecutive years).
- State of JS 2024 (Astro + 39 pts satisfaction over Next.js; Next.js retention softening).
- JavaScript Rising Stars 2024 and 2025 (Hono #2 backend 2024; Bun #1 build tool 2025; htmx #1 frontend 2024).
- DB-Engines ranking trends Q1 2025 (Postgres top climber in 4 of 6 months).
- ThoughtWorks Tech Radar Vol 31 and 34 (Svelte moved to Adopt Oct 2024; TanStack Start on Trial).
- State of Django 2025 (HTMX 24% / Alpine 14% adoption among Django devs).
- State of Laravel 2025 (Livewire 62% / Inertia 48%).
- Grafana Labs migration case studies (50+ Datadog to Grafana moves with ~40% savings).
- 37signals cloud exit savings updated from $7M to $10M+ over five years.
- Convex public migration of its own infra from Aurora MySQL to PlanetScale Postgres (2025).

### Did not change (explicitly)

- The 9-step workflow, 4-tier completion structure, and Mode A/B/C/D detection are unchanged.
- The have-nots list is unchanged; 1.1.0 additions would overlap existing disqualifiers.
- No new hard anti-pairings beyond sync-engine overlap; existing anti-pairings remain accurate.
- The compliance-as-filter principle is unchanged.

## [1.0.0] - 2026-04-22

Initial release. Third skill in the ready-suite alongside `production-ready` and `repo-ready`. Scope: stack selection. Does not build apps (that's `production-ready`) or configure repos (that's `repo-ready`).

### Added

- **Core `SKILL.md` with 10-step workflow** (Steps 0 through 9), 4 completion tiers (Shortlist, Scored, Justified, Decided), 18 requirements, per-step "Passes when" acceptance gates.
- **Four entry modes**: Greenfield (A), Assessment (B), Audit (C), Migration (D).
- **12 scoring dimensions** across framework, language, database, ORM, auth, UI, client state, hosting, observability, payments, email, and background jobs.
- **12 domain profiles** with full per-dimension picks: SaaS Multi-tenant, E-commerce / Retail, CMS / Content, Fintech / Financial, Healthcare / Medical, Education / LMS, CRM / Sales / Marketing, Customer Support / Helpdesk, Marketplace / Two-sided, AI / ML / LLM products, Analytics / BI, Internal Tools / Back-office.
- **Three bundle archetypes per domain**: Safe Default, Fast-to-Ship, Enterprise. 36 total pre-combined bundles.
- **Pairing compatibility rules** covering the most common anti-pairings (Convex + Prisma, Supabase + Firebase, multiple auth providers, multiple ORMs, multiple UI libraries, etc.).
- **Scoring framework** with published default weights, override mechanics, and per-score discipline rules (no 10/10 without consensus, no 1/10 without documented failure mode).
- **Tradeoff narrative discipline**: every shortlisted bundle ships with flip point, scale ceiling, and switching cost in engineer-weeks.
- **Migration path protocol** (Mode D) covering blast radius, sequencing, rollback checkpoints, data migration strategy, and honest team cost.
- **`.stack-ready/DECISION.md` artifact template** that slots directly into `production-ready`'s Step 2 architecture note.
- **`.stack-ready/STATE.md` session handoff template** for multi-session decisions (common in Mode C and Mode D).
- **Staleness protocol**: Step 9 prints skill version, last-updated date, current date, and warns if the skill is older than 6 months.
- **Have-nots list** of disqualifying anti-patterns (scores without stated weights, flip-point-free recommendations, ghost libraries, undisclosed commercial ties, etc.).
- **Reference library** loaded on demand:
  - `references/stack-research.md` - mode detection and codebase inventory protocol
  - `references/preflight-and-constraints.md` - 6 pre-flight questions and the constraint map
  - `references/domain-stacks.md` - 12 domain profiles with per-dimension picks
  - `references/stack-bundles.md` - 36 pre-combined bundles (3 archetypes x 12 domains)
  - `references/pairing-rules.md` - anti-pairings, overlap detection, substitution chains
  - `references/scoring-framework.md` - 12 dimensions, default weights, override mechanics
  - `references/dimension-deep-dives.md` - per-dimension analysis for close-call decisions
  - `references/tradeoff-narratives.md` - flip points, scale ceilings, switching costs per candidate
  - `references/migration-paths.md` - X-to-Y sequences for common migrations

### Principles

- **Score for this job, not this year.** Every score is opinionated under stated weights. Change the weights, the score changes. Weights are always surfaced.
- **No recommendation without a flip point.** Every shortlisted bundle names the failure mode that would reverse the choice.
- **No vendor capture.** No paid placement, no undisclosed commercial ties, no scores inflated by relationships.
- **Stops at the decision.** Does not build apps or configure repos. Composes with `production-ready` and `repo-ready`.
