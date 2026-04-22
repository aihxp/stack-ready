# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

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
