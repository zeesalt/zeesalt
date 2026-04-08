# Hi, I'm Z Kemecsei

Product manager tired of writing specs. Now I just build the product.

## What I Build
Full-stack apps and tools using AI-assisted development. 10+ years of enterprise product leadership, now shipping in the open with Claude Code as my engineering team.

## What I'm Working On
- 🏠 **Elevated BnB** — Multi-tenant vacation rental SaaS (founder)
- 🚴 Building side projects that scratch real itches
- 🤝 Open to PM roles at AI-first companies

## Featured Projects (See More Details for each project below)

| Project | What It Does | Stack |
|---------|-------------|-------|
| [Elevated BnB](https://app.elevated-bnb.com/) | Vacation rental operations platform — property onboarding, AI quality assessments, cleaning management, guest portals, direct bookings | TypeScript, Next.js, Supabase, Claude AI, Stripe |
| [TrailClear](https:/www.trailclear.com) | Mountain bike trail spring closure tracker for Boston-area parks | TypeScript, React |
| [HOA Tracker](https://hoa-tracker-gules.vercel.app) | Homeowners association activity tracker that saves admin busywork | JavaScript |

## Background
Most recently Product Lead of Mobile at Origami Risk, where I launched a 0→1 platform serving 200K+ users across 200+ enterprise accounts (~$7M net-new ARR). Built AI agents, product observability systems, and led cross-functional teams. Previously spent 7 years at PwC leading digital transformation for Fortune 500 insurance and financial services clients.

## Connect
- 💼 [LinkedIn](https://www.linkedin.com/in/zkemecsei/)
- 📧 [Email](mailto:zsolt.kemecsei@gmail.com)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
# TrailClear
**Live:** [trailclear.com](https://www.trailclear.com)
**Stack:** React 18, TypeScript, Vite, Tailwind CSS, Supabase, Vercel
**Role:** Solo — product, design, engineering

## The problem
Mountain bikers in the Northeast regularly drive 30–90 minutes to ride, only to find trails closed. Closure information is scattered across 30+ state park websites, NEMBA chapter Facebook groups, email newsletters, and word-of-mouth DMs. Each source uses different formats, update cadences, and reliability.

This causes three failures:
1. **Wasted trips** — riders drive an hour only to see a "closed" sign at the gate.
2. **Trail damage** — riders who can't find status information ride closed trails, causing erosion and straining relationships between land managers and the MTB community.
3. **Operator burden** — trail managers have no lightweight, trackable way to communicate closures beyond Facebook posts or CMS updates that most riders never see.

Existing apps (Trailforks, MTB Project) focus on trail mapping and navigation. None are built for the Northeast's specific closure patterns: formal spring mud closures, hunting season advisories, and "or as posted" policies that extend closures after storms.

## The approach
TrailClear is a trail-status PWA that consolidates closure information into a single, offline-capable source of truth. 176 parks across 11 states, updated every 6 hours.

Rather than asking operators to maintain yet another dashboard, the system encodes their **existing calendar rules as the default** — automating roughly 80% of closures. The operator dashboard exists for the remaining 20%: ad-hoc weather closures, storm damage, emergency hunting restrictions. This means the product is useful from day one, before any operator adopts it.

### Key decisions
**1. Calendar math over manual updates**
Most closures follow published rules. Blue Hills closes December 1 through April 15 every year. Wompatuck's hunting closure runs November 27 through December 10. Instead of building a workflow tool and waiting for operators to use it, I encoded 176 parks' closure rules as structured data. The system computes status from date logic, with operator overrides layered on top.

This was the core product insight: before building a workflow tool, check if the workflow can be eliminated. The 80/20 split meant the product delivered value immediately while the operator layer could grow organically.

**2. Static-first architecture**
All park data ships bundled in the build. No API call blocks the initial render. The app works fully offline at trailheads with no connectivity. Infrastructure cost is ~$1/month (Vercel + Supabase free tiers).

The tradeoff: adding a park requires a deploy. At 176 parks this is fine — a new park is a 15-line addition to a TypeScript file. The architecture has a planned migration to database-hosted parks at 500+, and the merge layer (`buildFullParkList`) that enables this is already in production.

**3. Operators as the distribution channel**
NEMBA chapter leaders and state park managers are the growth nodes. Each operator who adopts the dashboard brings their entire chapter's riders — a single adoption can onboard 1,000+ users overnight.
The operator experience is designed for minimal friction: claim a park, get approved, update status in seconds. Bulk condition imports let operators paste existing spreadsheets or email text. The system meets them where they already work instead of asking them to learn a new tool.

## What shipped
| Layer | What it does |
|-------|-------------|
| **Discovery** | ZIP-based proximity search, fuzzy text search, 6 filter dimensions, interactive map, suggested rides algorithm |
| **Park status** | Calendar-driven closure logic, operator overrides, community condition reports, scraped state park alerts |
| **Community** | Anonymous voting, trail chat (7-day TTL), riding-today check-ins, trail issue reporting, park follows |
| **Operator tools** | Status overrides, alerts, scheduled closures, events, bulk condition import, issue tracking |
| **Admin** | Park data management, operator onboarding, submission review, activity audit log, system announcements |
| **Data pipeline** | GitHub Actions scrapers (6h cycle) for MA/NH/NY state parks, NEMBA rides, bike shops, URL health checks |
| **Offline/PWA** | Installable app, service worker caching, offline action queue that auto-flushes on reconnect |

**By the numbers:** 176 parks, 11 states, 28 regions, 38 NEMBA chapters indexed, 129 passing tests, <2s time to first meaningful paint, ~$1/month total infrastructure cost.

## What v2 looks like
**Analytics, then growth.** The biggest gap is visibility into usage patterns. Which parks do riders check most? Where do they drop off? What's the conversion rate from "check status" to "drive to trailhead"? Basic instrumentation (region heat maps, time-to-open, funnel analysis) would clarify where the product adds the most value and where to invest next.

**Database-hosted parks.** The static park data approach breaks at scale. At 500+ parks, `parks.ts` adds 400KB+ to the bundle and every new park requires a deploy. The migration path is already partially built — `park_data_overrides` in Supabase can store full park records, and the merge layer is in production. Flipping to API-first means parks can be added, edited, and hidden without touching code.

**Embeddable status widgets.** A `?embed=parkId` card already exists. The next step is a polished, branded widget that NEMBA chapter websites and state park pages can drop in. This turns every partner into a distribution surface — riders discover TrailClear through the places they already check.

**National expansion.** The architecture is state-agnostic. The conditions scraper already supports `--state=XX` filtering for parallel execution. The closure-rule model works anywhere that has seasonal patterns. The path to 5,000+ parks is adding data and operators, not rebuilding infrastructure.
