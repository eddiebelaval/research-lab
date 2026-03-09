# Homer Platform — Executive Review Brief (Round 1)

## Subject
Full platform assessment of Homer (tryhomer.vip) — an AI-powered real estate platform with dual consumer (Homer Free) and B2B (Homer Pro) offerings.

## Module
`realestate`

## Panel Selection (Round 1)
1. **Managing Broker — Top-10 Brokerage** (operations, adoption, scale)
2. **Top-Producing Listing Agent — Luxury Market** (product quality, seller experience, competitive positioning)
3. **Proptech VC Partner — Series A/B Focus** (business model, unit economics, competitive moat)

## Rationale
Round 1 covers the three pillars that determine survival: Can agents adopt it? (Broker). Is the product good enough? (Agent). Is the business viable? (VC). Later rounds bring in legal, MLS, TC, and compliance perspectives.

## Platform Overview

### What Homer Is
- **Consumer side (Homer Free):** Homes have voices. Buyers don't browse static listings — they talk to houses. Each property assembles a personality from three knowledge layers (public data, seller narrative, agent privileged intel) and converses naturally using Claude Sonnet.
- **Agent side (Homer Pro):** Fully agentic AI admin for real estate agents. 78+ AI skills, deal state machine (discovery -> interest -> pursuit -> contract -> closing), deadline automation (Florida FAR/BAR), document management, communications hub, analytics.
- **Core IP:** Seller Interview Engine — conversational AI extracts lived experience from sellers (not a form). Coverage scoring across 6 domains (physical space, systems, community, neighborhood, lived experience, honest assessment). 10+ exchanges minimum, all domains touched.

### Technical Stack
- Next.js 16 + Supabase + Vercel (dashboard) + Railway (API)
- Fastify API with 18 route modules, rate limiting, CORS, security headers
- Claude Sonnet for all AI (voice synthesis, interviews, deal extraction, skill routing)
- pnpm monorepo: dashboard, api, widget, homer-pro, videos, 3 shared packages
- 40+ database indexes, 25 RLS policies, pgvector foundation
- Stripe billing (live since Mar 1, 2026)

### Current Status
- **Revenue:** $0. Pre-revenue.
- **Agents on platform:** 1 (Gus — first agent, testing full interview flow)
- **Transactions processed:** 0 real transactions. Deal state machine untested with real contracts.
- **Pipeline stage:** 9 of 11 (Launch Prep)
- **Build velocity:** 423 commits, 125 PRs, 660+ unit tests in 56 days
- **Team:** 1 founder (Eddie Belaval, product/vision/AI) + 1 engineering lead joining (Shah Asad Martinez, sweat equity)

### Revenue Model
| Tier | Price | Target |
|------|-------|--------|
| Homer Free | $0 | Buyers + sellers (lead magnet, brand awareness) |
| Homer Pro Core | $299-499/mo | Individual agents |
| Homer Pro Team | $999+/mo | Brokerages |

Metered usage tracking for deals closed, success fee calculations.

### Key Features Built
- Embeddable chat widget (Vite + React, responsive)
- Voice synthesis engine (3-layer knowledge assembly at query time)
- Seller interview engine (conversational, 6-domain coverage scoring)
- Deal extraction (natural language -> structured deal data via Claude)
- Transaction state machine (6 states, deadline tracking)
- Homer Pro workspace (9 Zustand stores, lazy-loaded panels)
- DocuSign, Twilio, SendGrid, Google Calendar integrations (built, not battle-tested)
- Vanity URLs for agent-specific workflows
- Stripe subscription billing with webhook handling

### Key Gaps
- No real transactions processed
- Florida-only contract/compliance logic
- No MLS integration (agent-provided data only)
- Shared Supabase instance (not dedicated)
- No staging environment
- No SOC 2 or security certification
- Minimal test coverage for Homer Pro
- Consciousness file upgrade planned but not implemented
- No mobile app (web responsive only)

### Competitive Landscape
- **Zillow/Redfin:** Consumer portals with agent tools. Massive distribution, no property voice.
- **Compass:** Premium brokerage platform. AI features growing. Luxury market focus.
- **KvCORE/Follow Up Boss:** CRM/lead management. No transaction management or property voice.
- **SkySlope/Dotloop/Open to Close:** Transaction management. No AI, no consumer side.
- **RealScout/HomeBot:** Buyer engagement tools. No seller narrative layer.

### The Thesis Being Evaluated
Homer's core thesis is that homes should be participants in their own sale — entities with voices that tell the truth — and that this creates value for all three sides: buyers get honest information, sellers get better representation of their home's character, and agents get deeper engagement and workflow automation. The question is whether this thesis translates into a viable product that agents will pay for and adopt.

## Evaluation Request
Tear this apart. Be the most skeptical version of your role. Assume nothing works until proven. Rate every dimension. Identify every gap. Then tell us exactly what to build to earn your money.
