# Foxhue Studio — Product Roadmap

*Internal strategy document for Andrew & Ashley*
*Last updated: 20 February 2026*

---

## 1. What We've Built

Foxhue Studio is a template-driven workflow that turns a single config file into a full set of client-facing deliverables. One repo per client, five commands, zero hardcoding.

### The Config: `project.json`

Every command reads from `project.json`. It holds:

| Section | What it controls |
|---------|-----------------|
| `agency` | Our details, email targets for form submissions |
| `client` | Name, tagline, industry, location, contact details, social URLs |
| `brand` | Colours, fonts, Google Fonts URL |
| `designs[]` | Design direction names and moods (array — supports 2, 3, or more) |
| `pages[]` | Every page in the website build (drives nav, variants hub, sitemap) |
| `deliverables` | Feature flags — toggles which reports and hub cards get generated |

No client data is ever hardcoded in commands, templates, or output files. Change the config, re-run, and everything updates. Portable across any client.

### The 5 Commands

| Command | Phase | What it produces | Dependencies |
|---------|-------|-----------------|--------------|
| `/scaffold` | Setup (Day 1) | Project hub, design directions hub, layout review hub, brand stylesheet, CLAUDE.md, asset brief, project status tracker — **9 files** | `project.json` only |
| `/seo-report` | Research (Days 2-3) | Branded HTML report: competitor profiles, gap analysis, keyword opportunities, technical recs, local SEO checklist | Perplexity MCP (live research) |
| `/social-audit` | Research (Days 2-3) | Branded HTML report: platform audit, competitor social analysis, content strategy, 3-month growth roadmap | Perplexity MCP (live research) |
| `/creative-process` | Design (Days 3-5) | Client-facing narrative page: the brief, research methodology, inspiration gallery, design principles, direction summaries | `inspiration/references.md` (manual curation) |
| `/landing-page` | Build (Days 5-10) | Complete landing page + 14 self-contained section blocks for Webflow embeds — **16 files** | Service name as argument |

### The 5-Phase Workflow

| Phase | Days | Key activities |
|-------|------|---------------|
| **1. Setup** | Day 1 | Fill `project.json` → run `/scaffold` → enable GitHub Pages → send asset brief to client |
| **2. Research** | Days 2-3 | Curate `references.md` → run `/seo-report` → run `/social-audit` (if applicable) |
| **3. Design** | Days 3-5 | Build 3 design directions → run `/creative-process` → share hub with client → client picks favourite |
| **4. Build** | Days 5-10 | Build all pages + layout variants → run `/landing-page` (if applicable) → client submits layout preferences via review hub |
| **5. Handoff** | Days 10-12 | Apply preferences → final polish → deliver (Webflow import, static, or handoff docs) |

### Client-Facing Deliverables

Everything we produce is branded and client-ready — not internal dev artifacts:

- **Project Hub** — single URL linking to all deliverables (4-6 cards depending on feature flags)
- **Creative Process** — narrative explaining our research and thinking behind design directions
- **Design Directions** — 3 branded homepage concepts for the client to choose from
- **Layout Review Hub** — per-page layout comparison with radio selectors, notes fields, and AJAX form submission
- **SEO Competitor Report** — real research, real competitors, scored gap analysis
- **Social Media Audit** — platform assessment, content strategy, and 3-month growth roadmap

### What's Proven: MARR Laser

MARR Laser & Skin Clinic is the first live project built on this system. It demonstrates the full workflow end-to-end:

- 10-page website with 3 layout variants per page (30 variant files)
- 14-section landing page for laser hair removal
- 3 design directions with a client review hub
- SEO report with real competitor data from Perplexity
- All deployed to GitHub Pages from a single repo
- Review hub with working AJAX form submission (Formsubmit.co → andrew@foxhue.com, CC ashley@foxhue.com)

The MARR repo is the reference implementation. Every pattern in the template was validated here first.

---

## 2. What's Working Well

**Single source of truth.** `project.json` eliminates copy-paste errors and makes the system portable. Clone the template, fill in the config, and every command outputs correctly branded, correctly linked files. No hunting through HTML to update a phone number or colour code.

**Everything is client-facing.** There are no internal-only outputs. The project hub, creative process page, review hub, and reports are all designed to be shared directly with clients. This makes us look polished without extra effort.

**Research-powered reports.** The SEO and social audit commands use Perplexity for live web research — real competitors, real data, real recommendations. Most agencies either skip this or do it manually. We automate it into branded, client-ready HTML reports. This is a genuine differentiator.

**Seamless client review.** The layout review hub uses AJAX form submission, localStorage auto-save, a live summary panel, and copy-to-clipboard fallback. The client never leaves the page, never loses their progress, and we get structured feedback (not scattered emails).

**Repeatable workflow.** The 5-phase structure with clear handoffs means we can onboard team members, estimate timelines, and maintain quality consistency across clients. It's not dependent on one person's memory.

**Self-contained landing page sections.** Each of the 14 landing page sections has its own `<style>` block and can be pasted into Webflow independently. This makes Webflow integration practical rather than theoretical.

---

## 3. Feature Roadmap

### Tier 1: Quick Wins
*Extend what already exists. Low effort, high value. Could build most of these in a single session.*

| Feature | What it does | Why it matters |
|---------|-------------|----------------|
| `/style-guide` | Auto-generate a visual style guide page from `project.json` brand tokens | Gives clients a tangible brand reference; useful for handoff to other vendors |
| `/schema-markup` | Generate JSON-LD structured data (LocalBusiness, Service, FAQ) from config | Immediate SEO value for every client; takes minutes to generate, hours to do manually |
| `/sitemap` | Generate `sitemap.xml` + `robots.txt` from the `pages[]` array | Basic SEO hygiene that should ship with every project |
| `brand.spacing` + `brand.radius` | Add spacing scale and border-radius tokens to the `project.json` schema | More design consistency without more manual CSS; small schema change, big visual impact |
| Industry templates | Pre-filled `project.json` files for common verticals (clinic, restaurant, professional services, trades, salon) | Faster project setup; reduces "blank page" friction; captures industry-specific page structures and content patterns |

**Effort estimate:** Each of these is 1-2 hours of build time. The schema additions are even less.

### Tier 2: Client Value-Adds
*New deliverables that increase what we can offer (and charge for).*

| Feature | What it does | Why it matters |
|---------|-------------|----------------|
| `/copy-brief` | Page-by-page copywriting briefs with SEO keywords, tone guidance, word counts, and section structure | Bridges the gap between design and content; gives copywriters (or clients writing their own copy) clear direction |
| `/content-calendar` | Extends the social audit into a monthly posting plan with post types, captions, hashtags, and scheduling recommendations | Turns a one-off audit into an ongoing deliverable; natural upsell |
| `/proposal` | Branded client pitch document — scope, timeline, deliverables, pricing, terms | Professionalises our sales process; pre-populated from `project.json` |
| `/google-business` | GBP audit and optimisation report — profile completeness, category accuracy, review strategy, post recommendations | High-value local SEO deliverable; most small business clients have neglected GBP profiles |
| Multi-landing-page support | Allow multiple service landing pages per project (e.g., laser hair removal + skin treatments) | Many clients need more than one landing page; currently the system assumes one |

**Business note:** `/copy-brief` and `/content-calendar` are natural add-ons that can be packaged into higher-tier offerings. `/proposal` saves us time on every new lead.

### Tier 3: Process Automation
*Speed up our internal workflow. Not client-facing, but reduces time per project.*

| Feature | What it does | Why it matters |
|---------|-------------|----------------|
| `/review-summary` | Parse client feedback (from review hub form email) into an actionable checklist with per-page tasks | Eliminates manual transcription of client preferences into task lists |
| `/handoff-pack` | Generate a branded handoff document — login credentials template, hosting details, DNS instructions, maintenance guide | Professional project close; reduces "what happens now?" conversations |
| `/deploy` | Automate GitHub Pages deployment, verify the site is live, return the URL | Removes the manual push + Pages enable + verify cycle |
| `/accessibility-check` | Playwright-powered WCAG compliance scan — contrast ratios, alt text, heading hierarchy, keyboard navigation | Quality assurance we should be doing anyway; can also be client-facing as proof of quality |

**Business note:** These don't generate revenue directly, but they reduce our cost per project. `/review-summary` alone could save 30-60 minutes per project.

### Tier 4: Premium Strategy Services
*Bigger bets. Higher margin. Position Foxhue as a strategic partner, not just a web builder.*

| Feature | What it does | Why it matters |
|---------|-------------|----------------|
| `/competitor-teardown` | Visual competitor analysis with Playwright screenshots, design critique, UX assessment, and positioning analysis | Demonstrates strategic depth; justifies premium pricing; clients love seeing their competitors dissected |
| `/brand-audit` | Brand positioning and perception research — online presence, review sentiment, visual consistency, messaging alignment | Moves us upstream from "build a website" to "shape your brand" |
| Retainer tooling | Monthly report templates, content performance tracking, automated check-ins | Recurring revenue; keeps clients engaged post-launch |

**Business note:** These features position us differently in the market. A `/competitor-teardown` in a proposal meeting changes the conversation from "how much does a website cost?" to "what's our competitive strategy?"

---

## 4. Business Considerations

### Internal Efficiency vs. Client-Facing Upsells

| Category | Features | Revenue impact |
|----------|----------|----------------|
| **Internal efficiency** (saves us time) | `/review-summary`, `/handoff-pack`, `/deploy`, industry templates, schema additions | Reduces cost per project; improves margins on existing pricing |
| **Included deliverables** (raises baseline quality) | `/style-guide`, `/schema-markup`, `/sitemap`, `/accessibility-check` | Differentiates our standard offering; justifies current pricing |
| **Upsell deliverables** (new revenue) | `/copy-brief`, `/content-calendar`, `/google-business`, `/proposal` | Can be packaged as add-ons or bundled into premium tiers |
| **Premium positioning** (changes the conversation) | `/competitor-teardown`, `/brand-audit`, retainer tooling | Justifies significantly higher project fees; recurring revenue |

### Pricing Implications

The current system already delivers more than most agencies at our price point. The question is whether to:

1. **Include more and charge the same** — use Tier 1 features to make the standard package more impressive (higher close rate, same price)
2. **Package into tiers** — Basic (current), Professional (+copy brief, +content calendar, +GBP audit), Premium (+competitor teardown, +brand audit)
3. **Add-on pricing** — keep the base package simple, sell reports individually

This is worth discussing — the right answer depends on our target client and market positioning.

### Competitive Advantage

What makes this different from other agency workflows:

- **Config-driven architecture.** Most agencies copy-paste and find-replace. We run a command and get consistent, error-free output. This scales.
- **Research-powered reports.** Live Perplexity research in branded reports is something most agencies don't offer at all, let alone automated.
- **Client experience.** The review hub with AJAX submission, auto-save, and live summary is noticeably better than "email us your feedback."
- **Reproducibility.** Any team member can run the same commands and get the same quality output. The system doesn't depend on one person's skill or memory.
- **Speed.** A full project setup (hub, asset brief, stylesheet, review hub, status tracker) takes minutes, not hours.

### Scalability

The template approach scales in three directions:

1. **Across clients.** Clone template → fill config → run commands. Adding a new client is fast and consistent.
2. **Across team members.** The workflow guide and command system mean a new team member can follow the process without extensive training.
3. **Across verticals.** Industry templates make it easy to adapt for different business types without rebuilding the system.

The main scaling constraint today is that design direction creation (Phase 3) is manual. Everything before and after it is largely automated.

---

## 5. Suggested Next Steps

### Immediate Priorities (Next Build Session)

These are ready to build now — no open questions, clear scope:

1. **`/style-guide`** — auto-generate from `project.json` tokens. Quick win, immediately useful, and makes the standard deliverable set feel more complete.
2. **`/schema-markup`** — JSON-LD from config. High SEO value for minimal effort.
3. **`brand.spacing` + `brand.radius`** — small schema additions that improve design consistency across all outputs.
4. **Industry templates** — start with clinic (from MARR) and one other vertical. Pre-filled `project.json` files that eliminate blank-page setup.

### Features Worth Discussing First

These need a conversation before committing:

- **`/copy-brief`** — What should the output format look like? Do we want this integrated with the SEO report keywords, or standalone? Who's the audience — us, a copywriter, or the client?
- **`/proposal`** — What goes in a Foxhue proposal? Scope template, pricing structure, terms? This needs Ashley's input on the sales process.
- **Pricing tiers** — Should we package features into tiers, or keep add-on pricing? This affects which features we prioritise building.
- **`/competitor-teardown`** — Big impact, but also the most complex build. Worth scoping the MVP before committing.

### Open Questions

- **Retainer model:** Do we want to offer ongoing services (monthly reports, content management)? If yes, the retainer tooling in Tier 4 becomes a higher priority.
- **Team scaling:** Are we planning to bring in contractors or junior staff? If yes, the process automation features (Tier 3) matter more and sooner.
- **Webflow vs. static:** How many clients will go through Webflow vs. static hosting? This affects how much we invest in Webflow-specific tooling.
- **Template versioning:** When we improve the template, how do we update existing client repos? This isn't urgent with one client, but will matter at five or ten.

---

*This document is a starting point for conversation, not a commitment list. Priorities should be reviewed together and adjusted based on what we're seeing from clients and the market.*
