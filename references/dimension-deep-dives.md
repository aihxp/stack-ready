# Dimension Deep Dives

Per-dimension analysis for close-call decisions. Loaded on demand in Step 5 when two candidates are within scoring range of each other and the user wants a deeper read.

**Scope owned by this file:** the tradeoffs inside each of the 12 dimensions: what separates the top candidates, the scoring axes that matter, the common misuses. Per-domain picks live in `domain-stacks.md`; scoring math lives in `scoring-framework.md`.

## 1. Framework

Fullstack meta-choice. Shapes everything below it.

### Top TypeScript candidates

- **Next.js 15.** App Router is now mature. Server Components + Server Actions collapse a large surface of API + client-state. Vercel is the default host. Ecosystem is the deepest. Score: 9 across most domains.
- **Remix v2+.** Nested routing, data loaders, strong web-platform alignment. Hot-reload DX is excellent. Smaller ecosystem than Next.js but recently reinvigorated. Score: 7-8 depending on domain.
- **SvelteKit.** Smallest-bundle story, excellent DX, solid data loading. Ecosystem is smaller for enterprise patterns (fewer packages for SAML, fewer Clerk-quality auth options). Score: 7-8 for teams that love Svelte; lower for teams that don't.
- **Nuxt.** Vue equivalent to Next.js. Strong in European markets. Score: 7-8.
- **Astro.** Content-first framework with partial hydration. Top pick for content-heavy sites. Score: 9 for content, 5 for app-heavy.

### Top non-TypeScript candidates

- **Rails 8 + Hotwire/Turbo.** Rails is back. Hotwire gives you SPA-like interactivity without a JS framework. DX is superb for CRUD-heavy work. Score: 9 in SaaS, Fintech, Education, CMS domains when the team has Ruby depth.
- **Django 5 + HTMX.** Mature, batteries-included, excellent admin. HTMX is a reasonable React alternative for moderate interactivity. Score: 8-9 for Python teams.
- **Phoenix LiveView.** Real-time native. Elixir's concurrency is a match for helpdesk, presence, collaborative editing. Score: 9 in real-time domains when the team has Elixir depth.
- **Laravel 11 + Inertia or Livewire.** PHP's most polished stack. Strong for SaaS with a JS or PHP-only interactive path. Score: 8.
- **FastAPI.** Not fullstack; pair with a separate frontend. Top pick when the backend is Python and the frontend is decoupled. Score: 8 as API layer.

### Scoring axes

- **DX.** How pleasant is day-to-day development?
- **Data flow.** How clean is server-to-client data handling?
- **Auth integration.** How well do top auth providers integrate?
- **Ecosystem.** Packages, tutorials, Stack Overflow depth.
- **Deployment story.** How well does it fit target hosting?
- **Concurrency model.** Matters for real-time, streaming, WebSockets.

### Common misuses

- Picking **Next.js for a content-only blog**: Astro or even a static site generator is simpler.
- Picking **Remix when the team has zero Remix experience** and the reason is "we want to try something new": the ecosystem difference will cost months.
- Picking **a JS framework when the team is Python-only**: Django is almost always the better bet.
- Picking **SvelteKit when enterprise SAML is in the pipeline**: the auth ecosystem is thinner; pair with a provider that ships a SvelteKit SDK.

---

## 2. Language / runtime

The language ripples through every other choice.

### Top candidates

- **TypeScript.** Default for fullstack JS. Type safety, huge ecosystem, first-class support across all major BaaS. Score: 9 for general-purpose web work.
- **Ruby.** Rails is the killer app. Excellent for CRUD-heavy work. Score: 9 for Rails teams.
- **Python.** Django is mature; FastAPI is modern. Dominates data/ML adjacency. Score: 8-9 depending on domain.
- **Elixir.** Concurrency is a superpower; Phoenix is polished. Score: 9 in real-time, 7 elsewhere.
- **Go.** Excellent for services and CLIs; weak for frontend-heavy fullstack. Score: 8 for APIs, 5 for fullstack web.
- **Kotlin / Java (Spring Boot).** Enterprise-native, deep ecosystem. Score: 8 for enterprise, over-engineered for SMB.
- **Rust (Axum, Actix).** Superb performance, steep learning curve. Score: 8 for infra/perf-critical, 5 for general web.
- **C# / .NET.** Underrated; excellent DX for Windows-adjacent orgs. Score: 8 in enterprise.

### Scoring axes

- **Team fit.** Does the team already know it, or are you hiring?
- **Ecosystem.** How many libraries are current?
- **Hireability.** How easy to hire for this language in 12 months?
- **Performance ceiling.** Does the language limit vertical scale?
- **Deployment options.** How many mature hosts exist?

### Common misuses

- Picking **Rust for a CRUD app** because it's fast: the team's velocity drop usually outweighs the runtime speedup.
- Picking **Go for frontend-heavy apps**: Go's template system is crude vs. TypeScript/Svelte/React.
- Picking **Elixir because the team lead is excited**: if the rest of the team doesn't know it, you are betting the product on learning.
- Picking **Python for high-concurrency real-time**: the GIL matters; async Python is viable but fighting the ecosystem.

---

## 3. Database

The longest-lived decision.

### Top candidates

- **Postgres.** The universal default. JSONB, full-text search, pgvector, PostGIS cover adjacent needs. Score: 10 in most domains.
- **MySQL.** Still dominant in some stacks (PlanetScale, Aurora, Rails-adjacent). Score: 7-8.
- **SQLite.** Surprisingly viable for small-to-medium production via LiteFS, Turso. Score: 6-8 depending on scale and deployment story.
- **Convex.** Managed, reactive, typed. Score: 9 for fast-to-ship SaaS; scale ceiling at cross-tenant SQL aggregations.
- **MongoDB.** Dominant in some use cases (CMS with flexible content), declining default. Score: 5-6 for transactional work; can be higher for specific document-natural data.
- **DynamoDB.** AWS-native, hyperscale. Score: 7 for AWS-deep teams; 5 for others.
- **ClickHouse.** OLAP-specialist; scale for analytics. Score: 10 for analytics dimensions, N/A for OLTP.
- **Firestore.** Firebase default; managed, real-time. Score: 7 for tiny apps, 5 for production scale (limits and pricing get painful).

### Scoring axes

- **Data model fit.** Relational, document, KV, graph?
- **Scale ceiling.** Where does it stop?
- **Ops burden.** Managed service tiers, self-host maturity.
- **Ecosystem.** ORMs, drivers, tooling.
- **Cost at scale.** Real cost at your 12-month projection.
- **Compliance.** BAAs, data residency, SOC 2.

### Common misuses

- Picking **MongoDB for relational data** because it "scales": joins are not actually a bottleneck at the scale most apps see.
- Picking **DynamoDB for a 3-person team** not on AWS: the ceremony is not worth it.
- Picking **Postgres but treating it like a NoSQL store** (no constraints, no foreign keys): you are paying Postgres costs for NoSQL benefits.
- Picking **SQLite for a 50-engineer team with heavy writes**: viable at smaller scale; fights at larger scale.

---

## 4. ORM / query layer

Team ergonomics and migration discipline.

### Top candidates

- **Drizzle.** TypeScript-first, SQL-transparent, type-safe. Rising default in 2025-2026. Score: 8-9.
- **Prisma.** Excellent DX, strong migration tooling. Score: 7-8. Slightly weaker for SQL-heavy queries.
- **Kysely.** Type-safe SQL query builder, minimal. Score: 7-8 for teams who want SQL-visible code.
- **SQLAlchemy.** Python mature default. Score: 8-9 for Python teams.
- **ActiveRecord.** Rails-native, most complete ORM overall for mature teams. Score: 9 in Rails.
- **Ecto.** Elixir's ORM; excellent. Score: 9 in Phoenix.
- **Raw SQL with a thin wrapper.** Always viable. Score: 7-8 when SQL fluency is strong; lower when it isn't.

### Scoring axes

- **Type safety.** Compile-time error catching.
- **Migration story.** How clean is schema evolution?
- **Escape hatches.** Can you drop to raw SQL for performance?
- **Maturity.** How production-proven?
- **Ecosystem.** Plugins, tutorials, Stack Overflow.

### Common misuses

- Picking **Prisma then fighting it on complex queries**: if you know 20% of your queries will be complex, Drizzle's SQL-visible model fits better.
- Picking **Drizzle when the team lacks SQL fluency**: Prisma or Drizzle Query API hides complexity; raw-style Drizzle requires reading generated SQL.
- Using **an ORM without understanding transactions**: regardless of ORM, transaction boundaries are where bugs live.

---

## 5. Auth / identity

Often the most churn-prone dimension.

### Top candidates

- **Better Auth.** TypeScript-native, self-hostable, excellent DX. Score: 9 for standard SaaS.
- **Clerk.** Best-in-class polished UI, Organizations, recent pricing changes are a concern at mid-scale. Score: 8-9 depending on MAU.
- **WorkOS (AuthKit).** Enterprise-first (SAML, SCIM, audit log export). Score: 10 when enterprise is in scope.
- **Auth.js (formerly NextAuth).** Long-standing TS auth. Score: 7; Better Auth is the modern successor for most use cases.
- **Supabase Auth.** Paired with Supabase DB, it's cohesive. Score: 8 in Supabase bundles.
- **Firebase Auth.** Paired with Firebase, fine. Score: 7.
- **Keycloak.** Open-source, self-hosted enterprise auth. Score: 7; heavy but complete.
- **Devise (Rails).** Mature, trusted. Score: 9 in Rails bundles.
- **django-allauth.** Same for Django. Score: 9.
- **Rolling your own.** Almost never the right call. Score: 3-5; the failure modes are well-documented.

### Scoring axes

- **Feature set.** Social login, MFA, passwordless, Organizations, SAML, SCIM.
- **Pricing at scale.** Per-MAU pricing cliffs.
- **Compliance.** BAAs, SOC 2, data residency.
- **UI polish.** How much do you want to build yourself?
- **Customization.** Can you theme, extend, override?
- **Exit story.** Can you export users if you switch?

### Common misuses

- Picking **Clerk for a 100k-MAU app** without pricing for the 100k-MAU tier: the bill lands in month 6.
- Picking **Auth.js in 2026** when Better Auth is the active project: you are choosing the less-maintained option.
- Picking **self-hosted Keycloak for a 3-person team**: ops burden exceeds the saving.
- Picking **WorkOS without enterprise prospects**: paying for SAML/SCIM you don't need.

---

## 6. UI library

Design velocity vs. design ownership.

### Top candidates

- **shadcn/ui + Radix primitives.** The modern TypeScript default. Copy-paste patterns; you own the code. Accessibility is built into Radix. Score: 9.
- **Radix alone (headless).** Maximum customization; slightly more boilerplate. Score: 8-9.
- **Mantine.** Batteries-included, excellent dashboard components. Score: 8.
- **Chakra UI.** Declining but mature. Score: 7.
- **MUI (Material-UI).** Enterprise-heavy, opinionated. Score: 7 for Google-style UIs, 5 when you want to brand.
- **Ant Design.** Similar to MUI but more common in enterprise APAC. Score: 7.
- **Tailwind UI.** Paid templates on top of Tailwind. Score: 7.
- **HeroUI (formerly NextUI).** Modern, well-designed. Score: 7-8.
- **Custom design system on Radix.** Enterprise default when brand matters. Score: 8; higher upfront cost.

### Scoring axes

- **Customization.** Can you make it look like your brand?
- **Accessibility.** WCAG AA out of the box?
- **Component completeness.** Does it have everything you need?
- **Theming.** CSS variables, design tokens, dark mode.
- **Ecosystem.** Tutorials, extensions, copycats.

### Common misuses

- Picking **shadcn/ui + MUI** in one app: they fight.
- Picking **Tailwind UI then customizing it to unrecognizability**: you've written a design system without the infrastructure.
- Picking **MUI then trying to make it look non-Material**: you will pay the "override" tax everywhere.
- Picking **a low-code UI kit for a product that needs brand differentiation**: every SaaS ends up looking the same.

---

## 7. Client state / data fetching

How the frontend talks to the backend.

### Top candidates

- **TanStack Query.** The mature default for client-side data fetching, caching, mutation. Score: 9.
- **SWR.** Vercel-authored, simpler than TanStack Query. Score: 7.
- **Server Components + Server Actions (Next.js 15+).** Reduces the need for client-side caches on many paths. Score: 9 when the app fits the pattern.
- **Convex hooks.** Reactive, tied to Convex. Score: 9 in Convex bundles.
- **Hotwire (Rails).** HTML-over-the-wire; replaces much of the client state layer. Score: 9 in Rails bundles.
- **HTMX (Django).** Similar. Score: 8-9 in Django bundles.
- **Phoenix LiveView.** Same idea, more sophisticated. Score: 9 in Phoenix bundles.
- **Apollo Client (GraphQL).** Score: 7. Use only if GraphQL is the API style.
- **RTK Query.** Score: 7. Use only with Redux Toolkit.
- **Raw `fetch` with useEffect.** Score: 4. You will rebuild TanStack Query badly.

### Scoring axes

- **Cache model.** Is invalidation explicit or automatic?
- **Optimistic updates.** Built-in or DIY?
- **DX.** How much boilerplate?
- **Integration.** With your framework's data flow?
- **Real-time support.** Subscriptions, websockets.

### Common misuses

- Using **useEffect + fetch for production data fetching**: you will rebuild caching, retry, deduplication badly.
- Pairing **TanStack Query with server-rendered data** without thinking about cache hydration: double-fetches or stale-until-revalidated visible to users.
- Using **Apollo Client when the backend isn't GraphQL**: overhead with no benefit.

---

## 8. Hosting / deploy

The hidden cost center.

### Top candidates

- **Vercel.** Next.js's home. Excellent DX, steep pricing at scale, egress and middleware invocations add up. Score: 8.
- **Netlify.** Jamstack-first, solid DX. Score: 7.
- **Railway.** Simple, generous. Score: 8.
- **Fly.io.** Persistent processes, WebSockets, geo-distribution. Score: 8 for those workloads.
- **Render.** Heroku-like simplicity. Score: 7.
- **Cloudflare Workers / Pages.** Edge-first; different runtime model. Score: 7; 8 for edge-suitable workloads.
- **AWS (ECS Fargate, EKS, App Runner).** Full control, high ops burden. Score: 8 for enterprise, 4 for solo.
- **GCP.** Similar. Score: 7.
- **Azure.** Enterprise in Microsoft shops. Score: 7.
- **Self-hosted VPS (Hetzner, Digital Ocean) with Dokku / Coolify / Kamal.** Score: 7 for cost-conscious teams.

### Scoring axes

- **Ops burden.** How many hours per week?
- **Cost at 12-month scale.** Egress, seats, invocations.
- **Deploy ergonomics.** Push to deploy, preview environments.
- **Compliance.** BAA, SOC 2, data residency.
- **Workload fit.** Serverless vs. persistent, edge vs. region.

### Common misuses

- Picking **Vercel without pricing for egress and seats**: the bill is a surprise.
- Picking **AWS for a 3-person team**: ops cost dominates.
- Picking **Cloudflare Workers for a workload that needs long-lived connections**: the runtime model fights.
- Picking **self-hosted Kubernetes** as a cost play for a small team: the ops cost erases the savings.

---

## 9. Observability

What you see when something breaks.

### Top candidates

- **Sentry.** Error tracking + session replay + performance. Score: 9.
- **Datadog.** Enterprise observability; everything in one place; expensive. Score: 8-9 at enterprise.
- **New Relic.** Mature, less modern DX. Score: 7.
- **Honeycomb.** Best-in-class tracing, wide events, excellent debugging UX. Score: 9 for teams that invest in tracing.
- **Axiom.** Log-first observability at reasonable cost. Score: 8.
- **Grafana Cloud.** Open-source stack managed. Score: 7.
- **Self-hosted Grafana + Prometheus + Loki + Tempo.** Score: 7 for ops-capable teams.
- **OpenTelemetry** (as a protocol choice): always yes for new projects; the destination still matters.

### Scoring axes

- **Error detection quality.** Grouping, context, release tracking.
- **Tracing depth.** Are you going to adopt traces, or just logs?
- **Cost at volume.** Per-host, per-GB, per-event pricing curves.
- **DX.** Ease of integration, dashboard UX.
- **Compliance.** BAA, data residency.

### Common misuses

- Shipping with **console.log as "observability"**: you will find out about a production bug from a customer.
- Paying for **Datadog on a 3-person team**: $10k/year could fund a better stack choice elsewhere.
- Using **three tools with overlapping scope**: Sentry + Rollbar + Datadog + New Relic is a procurement mistake.

---

## 10. Payments

Stripe-or-else.

### Top candidates

- **Stripe.** Universal default. Score: 10 for most use cases.
- **Stripe Connect.** Marketplace payments. Score: 10.
- **Stripe Tax.** Automated tax. Score: 9.
- **Adyen.** Enterprise-scale, global. Score: 8-9 at enterprise.
- **Braintree (PayPal).** Viable, narrower. Score: 7.
- **Paddle.** Merchant of Record; handles VAT for digital goods. Score: 8 for digital SaaS needing MoR.
- **Lemon Squeezy.** Paddle-lite, small-scale MoR. Score: 7.
- **Authorize.Net.** Legacy. Score: 5.
- **Rolling your own payment processing.** Score: 1. Do not.

### Scoring axes

- **Geographic coverage.** Where do your customers pay from?
- **Product model fit.** Subscription, one-time, usage-based, marketplace.
- **Merchant of Record.** Does the processor handle VAT/tax globally?
- **Fees.** Interchange, platform fees, connected accounts.
- **Integration depth.** SDK, webhook maturity.

### Common misuses

- Picking **Stripe with no webhook retry plan**: subscription state will diverge from reality.
- Picking **a non-MoR processor** while selling to EU B2C: you become the tax authority's problem.
- Building **custom subscription logic** on top of Stripe: Stripe Billing's primitives are more complete than people think.

---

## 11. Email / notifications

Deliverability is the hidden dimension.

### Top candidates

- **Postmark.** Transactional-first, excellent deliverability. Score: 9.
- **Resend.** Developer-first, growing fast, good DX. Score: 9.
- **SendGrid.** Mature, wider feature set, deliverability occasionally rough. Score: 7.
- **AWS SES.** Cheapest; requires careful DKIM/SPF/DMARC setup. Score: 7 for teams with deliverability expertise.
- **Loops.** Transactional and lifecycle. Score: 8.
- **Customer.io.** Lifecycle / marketing. Score: 9 for that job.
- **Paubox.** HIPAA-specialized. Score: 9 for healthcare.
- **Knock.** In-app + multi-channel notifications. Score: 9 for apps with complex notification preferences.
- **Novu.** Open-source Knock equivalent. Score: 8.

### Scoring axes

- **Deliverability.** Inbox rate; reputation management.
- **DX.** SDK, templates, API.
- **Cost at volume.** Per-email and per-seat pricing.
- **Compliance.** BAA, SOC 2.
- **Multi-channel.** Email + SMS + push + in-app.

### Common misuses

- Using **one provider for transactional and marketing**: marketing volume damages transactional deliverability.
- Picking **AWS SES without a deliverability plan**: bounces compound; you end up on blocklists.
- Using **Resend for healthcare** without checking BAA: may not be compliant.

---

## 12. Background jobs / queues

Where durability lives.

### Top candidates

- **Inngest.** Developer-first durable workflows; event-driven. Score: 9.
- **Trigger.dev.** Similar; direct competitor. Score: 8-9.
- **Temporal.** Enterprise-grade workflow engine; steep learning curve. Score: 9 for complex workflows.
- **BullMQ.** Redis-based, TypeScript-native queue. Score: 7.
- **SQS.** AWS-native. Score: 7; 8 in AWS-deep teams.
- **Sidekiq.** Rails-native. Score: 9 for Rails.
- **Celery.** Python default. Score: 8.
- **Oban.** Elixir-native, Postgres-backed. Score: 9 for Phoenix.
- **RabbitMQ.** Message broker, venerable. Score: 7.
- **Kafka.** Event streaming, different shape. Score: 7 for event-sourcing-native teams.

### Scoring axes

- **Durability.** Retries, dead-letter, idempotency.
- **Visibility.** Dashboard, tracing, alerts.
- **DX.** How easy to define a job?
- **Ops burden.** Hosted vs. self-host?
- **Fit to workload.** Short async vs. long-running vs. fan-out.

### Common misuses

- Running **jobs in the web process**: one slow job blocks web requests.
- Using **cron for everything**: no retry, no dashboard, silent failures.
- Picking **Temporal for simple async tasks**: ceremony dominates the benefit.
- Picking **BullMQ without Redis persistence**: jobs lost on restart.

## Cross-cutting dimensions

These dimensions interact heavily; keep an eye on interaction effects.

- **Auth + UI library**: Clerk provides complete UI; Better Auth needs you to build or use their headless.
- **Database + ORM**: Postgres + Drizzle is tight; MySQL + Drizzle works but has fewer patterns.
- **Hosting + framework**: Vercel + Next.js is near-symbiotic; Next.js on Render works but misses some DX.
- **Payments + domain**: Stripe Connect for marketplaces; Paddle for digital global SaaS; Adyen for enterprise.
- **Email + background jobs**: sending email reliably requires a retry-capable job system behind it.
- **Observability + scale**: error tracking (Sentry) fits all scales; tracing (Honeycomb) pays off at team scale and above.
