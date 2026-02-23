# Foxhue SuperClaude — Product & Commercialisation Strategy

*Internal strategy document for Andrew & Ashley*
*23 February 2026*

---

# Part 1: What foxhue-superclaude Is

## 1. The Pitch

Foxhue SuperClaude is a config-driven website delivery system. You clone a template repo, fill in one JSON file with the client's details, and run a set of slash commands — each one generates a specific, branded, client-ready deliverable. SEO reports, design directions, creative process pages, landing pages, layout review hubs — all produced from the same config, all consistent, all shareable via a single GitHub Pages URL. It turns a 2-week web project into a repeatable process that any team member can follow, and it makes a two-person agency look like it has a production department.

---

## 2. How It Works

### The architecture

Everything flows from one file: `project.json`. It holds the client's name, industry, location, brand colours, fonts, page list, design direction moods, and feature flags for which deliverables to generate. No client data is hardcoded in commands, templates, or output files.

The delivery model:

1. **Clone** the `foxhue-superclaude` template repo on GitHub
2. **Configure** `project.json` with the client's details
3. **Run commands** — each one reads the config and produces branded output files
4. **Deploy** via GitHub Pages — the client gets a single URL to access everything

### What `project.json` controls

| Section | What it controls |
|---------|-----------------|
| `agency` | Our details — emails for form submissions, GitHub org |
| `client` | Name, tagline, industry, location, contact details, social URLs |
| `brand` | Colours (5 tokens), fonts (heading + body), Google Fonts URL |
| `designs[]` | Design direction names and mood descriptions (supports 2, 3, or more) |
| `pages[]` | Every page in the website build (drives nav, variants hub, sitemap) |
| `deliverables` | Feature flags — toggles which reports and hub cards get generated |

Change the config, re-run, and everything updates. Portable across any client.

---

## 3. The Command Library

| Command | Phase | What it produces | Key dependency |
|---------|-------|-----------------|----------------|
| `/scaffold` | Setup (Day 1) | Project hub, design directions hub, layout review hub, brand stylesheet, CLAUDE.md, asset brief, project status tracker — **9 files** | `project.json` only |
| `/brand-palette` | Setup (Day 1) | Branded HTML report with 3 palette proposals (one per design direction), WCAG contrast checks, competitor colour audit, client feedback form | Perplexity MCP |
| `/seo-report` | Research (Days 2-3) | Branded HTML report: competitor profiles, gap analysis, keyword opportunities, technical recs, local SEO checklist | Perplexity MCP |
| `/social-audit` | Research (Days 2-3) | Branded HTML report: platform audit, competitor social analysis, content strategy, 3-month growth roadmap | Perplexity MCP |
| `/creative-process` | Design (Days 3-5) | Client-facing narrative page: the brief, research methodology, inspiration gallery, design principles, direction summaries | `inspiration/references.md` |
| `/landing-page` | Build (Days 5-10) | Complete landing page + 14 self-contained section blocks for Webflow embeds — **16 files** | Service name as argument |

Every command reads `project.json`. Every output is client-ready HTML — not internal dev artifacts.

---

## 4. The 5-Phase Workflow

| Phase | Timeline | What happens |
|-------|----------|-------------|
| **1. Setup** | Day 1 | Fill `project.json` → run `/scaffold` → enable GitHub Pages → send asset brief to client |
| **2. Research** | Days 2-3 | Curate design references → run `/seo-report` → run `/social-audit` (if applicable) |
| **3. Design** | Days 3-5 | Build 3 design directions → run `/creative-process` → share hub → client picks favourite |
| **4. Build** | Days 5-10 | Build all pages + layout variants → run `/landing-page` (if applicable) → client submits layout preferences via review hub |
| **5. Handoff** | Days 10-12 | Apply preferences → final polish → deliver (Webflow import, static, or handoff docs) |

The system handles Phases 1 and 2 almost entirely. Phase 3 (design direction creation) is the main manual bottleneck. Phases 4 and 5 are a mix of manual build and automated tooling.

---

## 5. Client-Facing Deliverables

What the client actually sees — all accessed from a single project hub URL:

| Deliverable | What the client experiences |
|------------|---------------------------|
| **Project Hub** | Single-page dashboard linking to all deliverables (4-6 cards depending on feature flags) |
| **Brand Palette Exploration** | 3 colour palette proposals with "in use" preview cards, contrast checks, and a feedback form |
| **SEO Competitor Report** | Real research on their local competitors — scored gap analysis, keyword opportunities, actionable recommendations |
| **Social Media Audit** | Platform assessment, competitor social analysis, content strategy with a 3-month roadmap |
| **Creative Process** | Narrative walkthrough of how we developed the design directions — shows our thinking, builds confidence |
| **Design Directions** | 3 distinct branded homepage concepts to choose from |
| **Layout Review Hub** | Per-page layout comparison (A/B/C) with radio selectors, notes fields, AJAX form submission, localStorage auto-save |

The client never sees a repo, a terminal, or a config file. They see a polished, branded experience at a GitHub Pages URL.

---

## 6. What's Proven

### MARR Laser & Skin Clinic — the reference implementation

MARR Laser is the first live project built on this system. It demonstrates the full workflow end-to-end:

- 10-page website with 3 layout variants per page (30 variant files)
- 14-section landing page for laser hair removal (self-contained Webflow-embeddable blocks)
- 3 design directions with a client review hub
- SEO competitor report with real Perplexity-researched data on local competitors
- Review hub with working AJAX form submission (Formsubmit.co → andrew@foxhue.com, CC ashley@foxhue.com)
- All deployed to GitHub Pages from a single repo

**Live**: https://ajeibbotson-cmyk.github.io/marr-laser/

### Foxhue's own brand work

The `/brand-palette` command was built and validated using Foxhue's own brand exploration — palette proposals, WCAG contrast calculations, competitor colour audit, client feedback form. The pattern works for our own brand decisions, not just client work.

---

## 7. What Makes It Different

| Advantage | Why it matters |
|-----------|---------------|
| **Config-driven architecture** | Most agencies copy-paste and find-replace. We run a command and get consistent, error-free output. This scales. |
| **Research-powered reports** | Perplexity MCP gives us live web research in branded reports. Most agencies don't offer automated competitor research at all. |
| **Seamless client review** | AJAX form submission, localStorage auto-save, live summary panel, copy-to-clipboard fallback. The client never leaves the page, never loses progress. |
| **Reproducibility** | Any team member can run the same commands and get the same quality output. The system doesn't depend on one person's skill or memory. |
| **Speed** | Full project setup (hub, asset brief, stylesheet, review hub, status tracker) takes minutes, not hours. |
| **Zero hardcoding** | Change the config, re-run. No hunting through HTML to update a phone number or colour code across 30 files. |

---

# Part 2: The Lead Magnet Funnel

## 8. The Core Idea

We already have the capability to generate branded, research-powered SEO reports at near-zero marginal cost. The `/seo-report` command does the heavy lifting — Perplexity runs the research, the command structures it into a professional HTML report.

The idea: offer a **free website audit** as a lead magnet targeting SMB owners who suspect their website isn't performing. Use a productised version of `/seo-report` (extended with GBP checks, schema audits, and page-level analysis) to deliver a genuinely useful audit — not a teaser with "call us for the full report."

The audit is the diagnostic. It tells them what's wrong. The fix — a new website, built by us — is the upsell.

**Why this works as a lead magnet:**

- It's genuinely valuable — the recipient gets real, actionable insights about their business
- It costs us almost nothing to produce (Perplexity research + automated report generation)
- It demonstrates competence — if the audit is good, they trust us to build the fix
- It creates urgency — seeing problems documented in a professional report motivates action
- It qualifies leads naturally — anyone who fills out the intake form is already thinking about their web presence

---

## 9. The Funnel — Stage by Stage

### Stage 1: Attract

**Lead magnet landing page.** Clean, single-purpose page.

- **Headline**: "Is your website costing you customers? Get a free audit."
- **Subhead**: "We'll analyse your site against your local competitors and show you exactly where you're losing ground."
- **CTA**: "Get My Free Audit" → intake form
- **Trust signals**: MARR case study screenshot, Foxhue branding, "No obligation, no sales call required"

**Traffic sources:**
- Paid social (Facebook/Instagram — target SMB owners, location-based)
- Organic content (LinkedIn posts about local SEO, website teardowns)
- Local networking (BNI, chamber of commerce — hand out the URL)
- Referrals from existing clients

### Stage 2: Qualify

**Short intake form (5-8 questions).** Captures enough data to both qualify the lead AND run the audit. See Section 10 for the full question list.

The form does double duty:
- **For us**: Tells us whether this lead is worth pursuing (budget, timeline, intent)
- **For the audit**: Provides the business name, URL, industry, and location needed to run `/free-audit`

### Stage 3: Deliver

**Auto-generate a branded audit report** from their answers.

- Run a productised version of `/seo-report` using data from the intake form
- Extended with GBP analysis, schema checks, and page-level "does this deserve to rank?" assessment (see Section 11)
- Foxhue-branded (not client-branded — they're not a client yet)
- Delivered by email as a link to a hosted HTML report

The report is the real deal — professional, genuinely useful, not a teaser. This is what builds trust.

### Stage 4: Follow Up

**Warm email sequence (3 emails over 7-10 days).**

| Email | Timing | Content |
|-------|--------|---------|
| **1. "Here's your audit"** | Immediately | Link to the report. Brief summary of the top 3 findings. "Take a look and let us know if you have questions." |
| **2. "3 things you could fix today"** | Day 3-4 | Pull the 3 quickest wins from the audit. Give them actual instructions they can follow themselves. This builds trust — we're helping, not hard-selling. |
| **3. "What your new site could look like"** | Day 7-10 | "We've put together 3 design directions for what your website could look like — at no cost." Link to their personalised design hub. This is the moment. |

### Stage 5: Hook

**3 free design directions** generated via foxhue-superclaude.

- We create a `project.json` from the intake form answers
- Run `/scaffold` to generate the infrastructure
- Build 3 design directions (the manual part — 2-3 hours of design work)
- Deploy to GitHub Pages
- Send the client a link to their personalised design hub

The client sees real, branded homepage concepts for their business. Not mockups in a PDF — live pages they can click through on their phone. This is where "interesting report" becomes "I want this."

### Stage 6: Convert

**Proposal for website build.**

- Generated via `/proposal` (from the roadmap — Tier 2 feature)
- Pricing tiers (see Section 13)
- Option for ongoing SEO retainer
- The audit and design directions have already demonstrated our capability — the proposal closes the loop

---

## 10. Qualification Questions

The intake form needs to capture enough to run the audit AND tell us whether the lead is worth the 2-3 hours of design work in Stage 5. Here are the questions with rationale:

| # | Question | Type | Data captured | Qualification signal |
|---|----------|------|--------------|---------------------|
| 1 | **What's your business name?** | Text (required) | Business identity for research | — |
| 2 | **What's your website URL?** | URL (required) | The site to audit | If they have no site, they need a build (hot lead) |
| 3 | **What industry are you in?** | Dropdown (required) | Industry context for competitor research | Helps us prioritise verticals we have templates for |
| 4 | **Where are you based?** | Text (required) | Location for local SEO research | Local businesses are our sweet spot |
| 5 | **What's not working with your current website?** | Textarea (required) | Pain points — what to focus the audit on | Reveals urgency and self-awareness of the problem |
| 6 | **Are you actively considering a new website or redesign?** | Radio: Yes, actively looking / Thinking about it / Just curious | Timeline intent | "Actively looking" = hot lead, "Just curious" = nurture |
| 7 | **What's your approximate budget for a new website?** | Radio: Under £2k / £2k-5k / £5k-10k / £10k+ / Not sure yet | Budget qualification | Filters out leads below our minimum |
| 8 | **Best email to send your audit to?** | Email (required) | Delivery address | — |

**Notes on question design:**
- Questions 1-4 populate the audit. They also make the form feel purposeful — "we need this to research your business."
- Question 5 gives us the emotional hook. Their words go into the audit intro: "You told us [X] wasn't working. Here's what we found."
- Question 6 is the primary qualifier. "Just curious" still gets the audit (builds goodwill), but we only invest in design directions for "actively looking" and "thinking about it."
- Question 7 filters on budget without being aggressive. "Not sure yet" is acceptable — budget clarity often comes after seeing the audit.

---

## 11. What the Free Audit Looks Like

The free audit extends the current `/seo-report` with additional sections drawn from the SEO prompts (the RTF file) that inspired the original MARR work. The diagnostic parts are free; the fixing and building parts become the upsell.

### Audit sections

| Section | Source | What it covers |
|---------|--------|---------------|
| **1. Your website vs competitors** | `/seo-report` competitor analysis | 3-5 local competitors identified via Perplexity. Business profiles, online presence scores, key strengths and weaknesses. Comparative gap analysis table. |
| **2. Google Business Profile check** | SEO prompts (GBP analysis) — maps to roadmap `/google-business` | Is their GBP claimed? Complete? Categories correct? Photos uploaded (real ones, not stock)? Review count and rating? Response rate? Post frequency? Compares against competitor GBP activity. |
| **3. Schema & technical snapshot** | SEO prompts (schema audit) — maps to roadmap `/schema-markup` | Does LocalBusiness JSON-LD exist? Is it useful or placeholder? Mobile-friendly? HTTPS? Basic page speed observations. Existing schema listed with a verdict. Missing high-priority schema identified. |
| **4. "Does your site deserve to rank?"** | SEO prompts (page-level reality check) | Take their top 2-3 service pages. Search the target keyword + location. Compare their page against the top 3 Google results for intent match, usefulness, local relevance, and trust signals. Give a verdict: deserves to rank higher / neutral / lower. One clear reason. One fix that would move rankings fastest. |
| **5. Quick-win keywords you're missing** | SEO prompts (low-hanging fruit keywords) + `/seo-report` keyword section | 10-15 high-intent local keywords they should be targeting. "Near me" variations. Emergency/urgency terms. Terms their competitors rank for that they don't. Prioritised by estimated impact. |
| **6. Your top 3 priorities** | Synthesis of sections 1-5 | The 3 things that would move the needle most for this specific business. Clear, actionable, jargon-free. |

### The CTA

At the bottom of every audit:

> **Want to see what your new website could look like?**
> We've put together 3 free design directions based on your brand and industry. No obligation — just a look at what's possible.
> [See Your Design Directions →]

### Branding

- Foxhue-branded (our colours, our logo, our fonts) — they're not a client yet
- Professional but not sterile — the tone is direct and helpful, like talking to a smart friend who happens to know SEO
- Attribution: "Prepared by Foxhue Ltd"
- Hosted on GitHub Pages at a unique URL per lead

### What this becomes technically

A new command: `/free-audit` that:

- Reads from a simplified `project.json` (populated from the intake form)
- Runs Perplexity research (same pattern as `/seo-report`)
- Adds GBP analysis, schema check, and page-level reality check sections
- Outputs a Foxhue-branded HTML report (not client-branded)
- Includes the CTA linking to their design directions hub

### The diagnostic/upsell split

| Free audit covers (diagnostic) | Upsell covers (the fix) |
|-------------------------------|------------------------|
| Competitor landscape | Website build that outperforms competitors |
| GBP completeness check | GBP optimisation and management (retainer) |
| Schema audit — what's missing | Schema markup implementation (`/schema-markup`) |
| Page-level ranking reality check | New pages built to rank (`/copy-brief`, money pages) |
| Keyword gaps | Content strategy and creation (`/content-calendar`) |
| Top 3 priorities | Full implementation of those priorities |

The audit shows problems. We sell the solutions.

---

## 12. The 3 Free Designs Offer

### How it works operationally

1. **Create project.json** from the intake form answers (business name, industry, location, URL → mapped into config fields)
2. **Run `/scaffold`** to generate the project infrastructure (project hub, design directions hub)
3. **Research** — quick Perplexity pass on industry aesthetics and competitor sites (can reuse audit research)
4. **Build 3 design directions** — this is the manual part. 3 distinct homepage concepts for their business.
5. **Deploy** to GitHub Pages
6. **Send the link** — personalised design hub at `https://ajeibbotson-cmyk.github.io/{client-slug}/designs/`

### What the client sees

A branded design hub with 3 distinct homepage concepts they can click through. Not flat mockups in a PDF — live HTML pages that work on their phone. Each direction has a different visual personality (same pattern we used for MARR: Serene Spa / Editorial Bold / Warm Boutique, adapted to their industry).

### Time investment per lead

| Step | Time | Automated? |
|------|------|-----------|
| Create project.json | 10 min | Could be automated from form data (stretch goal) |
| Run `/scaffold` | 2 min | Yes |
| Research pass | 15 min | Partially (Perplexity) |
| Build 3 design directions | 2-3 hours | No — this is the bottleneck |
| Deploy + send link | 5 min | Yes |
| **Total** | **~3 hours** | **~80% automated except design** |

### When to invest the 3 hours

Not every lead gets design directions. The qualification logic:

- **"Actively looking" + budget £2k+** → Full treatment: audit + designs
- **"Thinking about it" + budget £2k+** → Audit first, designs after they engage with it
- **"Just curious" or budget under £2k** → Audit only (still builds goodwill, may convert later)
- **No website at all** → Skip the audit, go straight to designs (they need a build, not a diagnosis)

---

## 13. Upsell Path and Pricing

### The tiers

| Tier | What they get | Price | Notes |
|------|--------------|-------|-------|
| **Free** | Website audit + 3 design directions | £0 | Lead magnet. Cost to us: ~3 hours. |
| **Website Build** | Full site (up to 10 pages + 3 layout variants per page + landing page) | TBD | Includes all Phase 1-5 deliverables: project hub, SEO report, creative process, design directions, review hub, landing page. |
| **Website + SEO Retainer** | Full build + monthly SEO reporting, content recommendations, GBP management | TBD (build fee + monthly) | Recurring revenue. Retainer deliverables powered by roadmap commands. |

### How roadmap features feed into the retainer

| Roadmap feature | Retainer deliverable |
|----------------|---------------------|
| `/content-calendar` | Monthly content plan with post types, captions, hashtags, scheduling |
| `/google-business` | Monthly GBP optimisation — posts, photos, review strategy, category updates |
| Retainer tooling (Tier 4) | Monthly performance reports, automated check-ins, content tracking |
| `/competitor-teardown` | Quarterly competitor analysis — what's changed, new threats, new opportunities |
| `/copy-brief` | Ongoing page-level copywriting briefs as we add content |

### Pricing considerations

Pricing isn't locked yet, but the framework:

- **Build pricing** should account for the fact that the free audit and designs already demonstrate our capability. The client has seen the quality before they get a quote. This reduces price sensitivity.
- **Retainer pricing** should be monthly, with a minimum commitment (3 or 6 months). The tooling makes it efficient for us to deliver — the margin should be strong.
- **The free tier is an investment**, not a loss leader. 3 hours per qualified lead is sustainable if the conversion rate to paid work is reasonable. At even a 20% conversion rate, that's 15 hours of free work for every paid project — acceptable if project values are £3k+.

---

## 14. What We Need to Build

Concrete list of new assets and commands needed to make the funnel operational:

| Asset | Description | Effort | Dependencies |
|-------|------------|--------|-------------|
| **Lead magnet landing page** | Single-purpose HTML/CSS page. "Is your website costing you customers?" headline, intake form, trust signals. Can use the `/landing-page` command pattern. | 1 day | None — can build now |
| **Intake form** | 8-question form (see Section 10). Options: Formsubmit.co (simple, free, matches our existing pattern) or Typeform (nicer UX, paid). Needs to capture data we can feed into `project.json`. | Half day | Landing page |
| **`/free-audit` command** | Lighter variant of `/seo-report` extended with GBP check, schema audit, and page-level ranking assessment. Foxhue-branded output (not client-branded). | 2-3 days | `/seo-report` as base |
| **Email templates** | 3 HTML email templates for the follow-up sequence (audit delivery, quick wins, design directions offer). Plain but branded. | 1 day | Audit command |
| **Proposal template** | Branded pitch document — scope, timeline, deliverables, pricing. Maps to the `/proposal` item in the roadmap. | 1-2 days | None — can build alongside |
| **Form → project.json automation** | Script to create a `project.json` from intake form submission data. Stretch goal — can do this manually at first. | 1 day | Intake form |

### Build priority order

1. **`/free-audit` command** — the core product of the funnel. Build and test this first.
2. **Landing page + intake form** — the front door. Keep it simple.
3. **Email templates** — needed for delivery. Can be plain text initially.
4. **Proposal template** — needed to close. Build when the first leads come through.
5. **Form → project.json automation** — nice to have. Do manually until volume justifies it.

---

## 15. Risks and Open Questions

### The bottleneck: design direction creation

The 3 free design directions take 2-3 hours of manual design work per lead. This is the constraint on the whole funnel. If we get 10 qualified leads a week, that's 20-30 hours of design work — more than one person can handle alongside paid projects.

**Mitigation options:**
- Gate the designs behind a quick call (5-10 min video chat). This further qualifies and reduces wasted design time. But it also adds friction.
- Build industry-specific design direction templates (clinic template, restaurant template, etc.) that reduce the manual work per lead to 1-1.5 hours.
- Only offer designs to "actively looking" + budget-qualified leads (the qualification logic in Section 12).
- Hire a junior designer to handle design directions from templates once volume justifies it.

### Gate or send cold?

Do we require a call before sending design directions, or send them unprompted?

- **Send cold**: Lower friction, higher volume, more "wow" factor (they didn't expect this). Risk: wasted effort on leads that were never going to convert.
- **Gate behind a call**: Better qualification, personal connection, can tailor directions to the conversation. Risk: drop-off at the call-booking stage.

**Recommendation**: Start by sending cold to hot leads (actively looking + budget £2k+). Track conversion. If the conversion rate is low, add a call gate. We can always tighten later — harder to loosen.

### Volume scaling

If the funnel works and we're getting 10+ qualified leads per week:

- Design direction creation becomes the bottleneck (see above)
- We need reliable infrastructure for hosting multiple lead projects on GitHub Pages
- Email delivery needs to be more robust than manual sending (consider a simple email service)
- This connects to the team scaling question in the product roadmap — contractors or junior staff

### GDPR compliance

We're collecting personal data (name, email, business details) and sending follow-up emails.

**Requirements:**
- Privacy policy on the landing page
- Explicit consent checkbox on the intake form ("I agree to receive my free audit and follow-up emails from Foxhue")
- Easy unsubscribe in every email
- Data retention policy (how long we keep lead data)
- If using Formsubmit.co, check their data processing terms

### Vertical-specific vs generic landing page

**Option A: One generic landing page** — "Is your website costing you customers?" Works for any SMB.

**Option B: Vertical-specific pages** — "Is your clinic's website costing you patients?" / "Is your restaurant's website costing you bookings?" More targeted, higher conversion, but more pages to build and more ad spend to split across them.

**Recommendation**: Start generic. If a particular vertical converts well (e.g., clinics, given the MARR experience), build a vertical-specific variant for that industry. Data-driven expansion, not speculative.

---

## 16. Map to Existing Roadmap

The product roadmap (see `docs/product-roadmap.md`) already has features planned that directly support this funnel. Here's how they connect:

### Tier 1 (Quick Wins) → Faster Design Turnaround

| Roadmap feature | Funnel impact |
|----------------|---------------|
| `/style-guide` | Auto-generates a visual style guide from config. Speeds up design direction creation. |
| Industry templates | Pre-filled `project.json` for common verticals. Reduces "blank page" setup for each lead to minutes. Directly reduces the 2-3 hour design bottleneck. |
| `/schema-markup` | Feeds into the free audit's schema section. Also an immediate deliverable for paying clients. |

### Tier 2 (Client Value-Adds) → Upsell Deliverables

| Roadmap feature | Funnel impact |
|----------------|---------------|
| `/proposal` | Branded pitch document for Stage 6 (Convert). Pre-populated from `project.json`. |
| `/copy-brief` | Upsell deliverable — bridges the gap between design and content. Natural add-on to the website build. |
| `/google-business` | Powers the GBP section of the free audit. Also an ongoing retainer deliverable. |
| `/content-calendar` | Retainer deliverable. Turns the social audit into a recurring monthly plan. |

### Tier 3 (Process Automation) → Handle Volume

| Roadmap feature | Funnel impact |
|----------------|---------------|
| `/deploy` | Automates GitHub Pages deployment. Critical if we're deploying 10+ lead projects per week. |
| `/review-summary` | Parses client feedback into task lists. Saves 30-60 minutes per converting lead. |
| `/handoff-pack` | Professional project close. Reduces post-build admin for every paying client. |

### Tier 4 (Premium Strategy Services) → Premium Tier and Recurring Revenue

| Roadmap feature | Funnel impact |
|----------------|---------------|
| `/competitor-teardown` | Premium upgrade to the free audit. Visual competitor analysis with screenshots and UX critique. Justifies higher project fees. |
| `/brand-audit` | Moves us upstream from "website builder" to "brand strategist." Changes the conversation at the proposal stage. |
| Retainer tooling | Monthly reports, content tracking, automated check-ins. The infrastructure for recurring revenue. |

### The virtuous cycle

The funnel and the roadmap reinforce each other:

- Building funnel infrastructure (free audit, landing page, email templates) validates which roadmap features matter most
- Each roadmap feature we ship makes the funnel cheaper to run or more valuable to the lead
- Client projects funded by the funnel generate revenue to invest in roadmap development
- Industry templates built for paying clients reduce the cost of the free design directions for that vertical

---

## Summary: The Path from Stranger to Retainer Client

```
Stranger sees ad/post
        ↓
Landing page: "Is your website costing you customers?"
        ↓
Intake form (8 questions) — qualifies AND captures audit data
        ↓
/free-audit runs → branded report delivered by email
        ↓
Email 2: "3 things you could fix today" (builds trust)
        ↓
Email 3: "We've built 3 design directions for you" (the hook)
        ↓
Client sees live homepage concepts on GitHub Pages
        ↓
Proposal: website build + optional SEO retainer
        ↓
Paying client → full 5-phase SuperClaude workflow
        ↓
Retainer: monthly SEO, GBP management, content strategy
```

Every stage is powered by foxhue-superclaude commands. The same system that delivers the free audit delivers the paid website. The tooling that hooks the lead is the tooling that serves the client.

---

*This document sits alongside `product-roadmap.md` (feature pipeline) and `workflow-guide.md` (operational process). It's the commercial strategy layer — how we turn the product into revenue.*
