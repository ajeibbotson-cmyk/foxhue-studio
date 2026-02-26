# Foxhue Project Workflow Guide

Step-by-step process for delivering a new client website project using the Foxhue Studio template.

---

## Phase 1: Setup (Day 1)

### 1.1 Create the project repo
- Go to [foxhue-studio](https://github.com/ajeibbotson-cmyk/foxhue-studio) on GitHub
- Click **"Use this template"** → **"Create a new repository"**
- Name it `{client-slug}` (e.g., `marr-laser`)
- Set to **Public** (required for GitHub Pages)
- Clone locally

### 1.2 Configure project.json
Fill out every field in `project.json`:

| Section | What to fill in | Where to get it |
|---------|----------------|-----------------|
| `agency` | Already set — verify emails | Internal |
| `client` | Name, tagline, slug, industry, location, contact | Client brief / intake call |
| `client.social` | Social media profile URLs | Client or manual search |
| `brand.mood` | (Optional) mood keywords if client has strong colour preferences | Client conversation |
| `brand.colors` | Primary, accent, background, surface | Brand guidelines PDF, or run `/brand-palette` to generate suggestions |
| `brand.fonts` | Heading + body fonts, Google Fonts URL | Brand guidelines PDF |
| `designs` | 3 direction names and mood descriptions | Decide during research phase (can update later) |
| `pages` | List of website pages | Sitemap from planning |
| `deliverables` | Feature flags for which reports to generate | Project scope |

### 1.3 Run /brand-palette (if no style guide)
If the client doesn't have a brand guidelines PDF with defined colours, generate palette suggestions:
```
/brand-palette
```
This researches the client's current site and competitor aesthetics via one Perplexity query, then derives palettes from each direction's mood using colour theory. Generates `docs/brand-palette.html` — a client-ready report with 3 distinct palette proposals (one per design direction).

After reviewing the proposals:
- Pick a palette (or mix elements from multiple proposals)
- Copy the chosen hex values into `project.json` → `brand.colors`
- If the client has a style guide PDF, skip this step and fill `brand.colors` directly from the PDF

### 1.4 Run /scaffold
```
/scaffold
```
This generates:
- `index.html` — Project hub (4-card layout: Creative Process, Design Directions, Layout Review, SEO Report)
- `designs/index.html` + `designs/styles.css` — Design directions hub
- `website/styles.css` — Brand tokens as CSS custom properties
- `website/variants/index.html` — Client review hub (AJAX form)
- `website/variants/thank-you.html` — Form confirmation page
- `CLAUDE.md` — Project context for Claude Code
- `docs/client-asset-brief.md` — Asset request checklist
- `docs/project-status.md` — Deliverables tracker

### 1.5 Send asset brief
- Send `docs/client-asset-brief.md` to the client (email or shared doc)
- Request brand guidelines, photos, copy, testimonials, social links

### 1.6 Enable GitHub Pages
- Repo **Settings** → **Pages** → Source: **Deploy from branch** → Branch: **main**, folder: **/ (root)**
- Share the hub link: `https://ajeibbotson-cmyk.github.io/{client-slug}/`

---

## Phase 2: Research (Days 2-3)

### 2.1 Design inspiration research
- Research 10-12 websites relevant to the client's industry and aesthetic
- Populate `inspiration/references.md` with each reference:
  - URL, site name
  - Category (agency, studio, competitor, aspirational)
  - What's effective (layout, typography, colour, interaction)
  - Which design direction it might inspire (tag with A, B, or C)
  - What to borrow or adapt

### 2.2 SEO competitor report
```
/seo-report
```
- Uses Perplexity to find 3-5 local competitors
- Deep analysis of each competitor's SEO, content, and local presence
- Generates `docs/seo-competitor-report.html` — a client-ready branded report
- Review the report and extract strategic insights for design/content decisions

### 2.3 Social media audit (if applicable)
```
/social-audit
```
- Requires `client.social` URLs in `project.json` (or searches for presence)
- Competitor social analysis via Perplexity
- Platform recommendations and content strategy
- Generates `docs/social-media-audit.html`
- Only run if `deliverables.social_audit` is `true`

### 2.4 Review research findings
- Read both reports
- Identify patterns: what works in this market, what's missing, where the opportunities are
- Use insights to refine design direction names and moods in `project.json`
- If research changes the mood keywords or direction moods, re-run `/brand-palette` to update palette proposals
- Update `docs/project-status.md` — tick off research phase items

---

## Phase 3: Design Directions (Days 3-5)

### 3.1 Build 3 design directions
For each direction (A, B, C):
- Create `designs/design-{key}/index.html` + `designs/design-{key}/styles.css`
- Each direction should present a distinct visual approach to the client's homepage
- Use brand tokens from `project.json`
- Reference images from `website/images/`
- Each design is self-contained HTML/CSS

### 3.2 Generate creative process page
```
/creative-process
```
- Reads `project.json` + `inspiration/references.md`
- Generates `docs/creative-process.html` — a narrative walkthrough of how directions were developed
- Brief → Research → Inspiration → Principles → Directions
- Run this **after** references.md is complete and designs are built

### 3.3 Deploy and share
- Commit and push all changes
- Share the project hub link with the client
- Client journey: **Creative Process** → **Design Directions** → picks favourite direction

### 3.4 Client review
- Client reviews the creative process page to understand the thinking
- Client reviews all 3 design directions
- Client picks their preferred direction (becomes Layout A for the full build)
- Update `docs/project-status.md`

---

## Phase 4: Full Build (Days 5-10)

### 4.1 Build website pages (Layout A)
- Build all pages listed in `project.json` `pages[]` array
- Use the client's chosen design direction as Layout A
- Pages go in `website/` (e.g., `website/index.html`, `website/about.html`)
- Share stylesheet: `website/styles.css`
- Images in `website/images/`

### 4.2 Build layout variants (B and C)
- For each page, create B and C layout alternatives in `website/variants/`
- Naming: `{slug}-b.html`, `{slug}-c.html` (e.g., `about-b.html`, `about-c.html`)
- Each variant offers a different layout approach for the same content
- These reference `website/styles.css` for shared brand tokens

### 4.3 Build landing page (if applicable)
```
/landing-page {service name}
```
- Generates `landing/index.html` + section blocks in `landing/sections/`
- Self-contained sections for Webflow embeds
- BEM naming with client slug prefix
- Only run if `deliverables.landing_page` is `true`

### 4.4 Client layout review
- Push all changes
- Share the review hub link: `website/variants/index.html`
- Client selects preferred layout (A, B, or C) per page
- Client adds notes per page
- Client submits form (goes to agency email via Formsubmit.co)
- Update `docs/project-status.md`

---

## Phase 5: Handoff (Days 10-12)

### 5.1 Apply client preferences
- Review the form submission (check email)
- Apply the client's per-page layout choices
- Address any notes or change requests

### 5.2 Final polish
- Cross-browser check
- Responsive testing
- Image optimisation
- Link validation
- Content proofread

### 5.3 Deliver
Choose delivery method:
- **Webflow**: Export sections via Moden import
- **Static site**: Deliver the `website/` directory as-is
- **Handoff docs**: Generate any additional documentation needed

### 5.4 Close project
- Update `docs/project-status.md` — mark all items complete
- Final commit and push
- Archive or transfer repo as needed

---

## Command Reference

| Command | When to run | Output |
|---------|-------------|--------|
| `/scaffold` | Day 1, after filling project.json | All infrastructure files |
| `/brand-palette` | Day 1, before scaffold (if no style guide) | `docs/brand-palette.html` |
| `/seo-report` | Research phase | `docs/seo-competitor-report.html` |
| `/social-audit` | Research phase (if applicable) | `docs/social-media-audit.html` |
| `/creative-process` | After research + designs built | `docs/creative-process.html` |
| `/landing-page {service}` | Build phase (if applicable) | `landing/` directory |

## File Reference

| File | Purpose | Generated by |
|------|---------|-------------|
| `project.json` | All client config | Manual (template provided) |
| `index.html` | Project hub | `/scaffold` |
| `designs/index.html` | Design directions hub | `/scaffold` |
| `designs/styles.css` | Shared design styles | `/scaffold` |
| `website/styles.css` | Brand tokens CSS | `/scaffold` |
| `website/variants/index.html` | Client review hub | `/scaffold` |
| `website/variants/thank-you.html` | Form confirmation | `/scaffold` |
| `CLAUDE.md` | Project context | `/scaffold` |
| `docs/client-asset-brief.md` | Asset request checklist | `/scaffold` |
| `docs/project-status.md` | Deliverables tracker | `/scaffold` |
| `docs/brand-palette.html` | Colour palette proposals | `/brand-palette` |
| `docs/seo-competitor-report.html` | SEO analysis | `/seo-report` |
| `docs/social-media-audit.html` | Social audit | `/social-audit` |
| `docs/creative-process.html` | Creative process | `/creative-process` |
| `landing/index.html` | Landing page | `/landing-page` |
| `landing/sections/*.html` | Landing sections | `/landing-page` |
| `inspiration/references.md` | Design research | Manual (template provided) |
