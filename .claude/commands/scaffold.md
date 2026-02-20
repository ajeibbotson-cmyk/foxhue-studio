# /scaffold — Generate project infrastructure from project.json

Read `project.json` from the repo root and generate all scaffolded files. Every generated file must pull its values (names, colors, fonts, emails, pages, designs) from the config — never hardcode client-specific data.

## Steps

### 1. Read project.json
Parse the config file at the repo root. Extract:
- `agency.*` — agency name, emails, GitHub org
- `client.*` — client name, short name, tagline, slug, industry, location, phone, email
- `client.social.*` — social media profile URLs
- `brand.colors.*` — primary, accent, accent_hover, background, surface
- `brand.fonts.*` — heading, body, google_fonts_url
- `designs[]` — array of { key, name, mood }
- `pages[]` — array of { key, title, slug }
- `deliverables.*` — feature flags (seo_report, creative_process, social_audit, landing_page)

### 2. Generate root index.html (project hub)
**File**: `index.html`

A branded Foxhue Project Hub linking to all deliverables. This is the main entry point shared with clients and used internally.

Requirements:
- Title: `{client.name} — Project Hub`
- Header: "Welcome to the Foxhue Project Hub" as the main heading
- Below the heading, a horizontal info bar with three columns (separated by top/bottom border lines):
  - **Client** → `{client.name}`
  - **Industry** → `{client.industry}`
  - **Location** → `{client.location}`
  - Each column has a small uppercase label above the value
  - Values use the heading font, labels use small uppercase body font in accent colour
  - On mobile, the three columns stack vertically
- Welcome heading uses the **body font** (not heading font) — clean, modern, sans-serif
- **Numbered card grid** (2 columns, stacks to 1 on mobile) linking to deliverables:
  1. **Creative Process** → `docs/creative-process.html` — "How we got here — the research, inspiration, and thinking behind your design directions"
  2. **Design Directions** → `designs/index.html` — "Three distinct visual approaches for {client.short_name}"
  3. **Layout Review** → `website/variants/index.html` — "Compare all three layouts across every page, pick your preferences, and leave notes"
  4. **SEO Report** → `docs/seo-competitor-report.html` — "In-depth competitor analysis, keyword opportunities, and SEO recommendations"
- Each card has: number (01, 02...), heading (heading font), description, and uppercase arrow link text
- Cards have rounded corners, subtle shadow, hover lift effect with increased shadow
- Wrap cards in a `<nav>` with `aria-label="Project deliverables"`
- **Conditional cards** based on `deliverables` flags:
  - If `deliverables.social_audit` is `true`, add a 5th card: **Social Audit** → `docs/social-media-audit.html`
  - If `deliverables.landing_page` is `true`, add a card: **Landing Page** → `landing/index.html` — "Campaign landing page for Meta advertising — standalone offer with pricing and booking"
- Footer: `Prepared by {agency.name}`
- Add `<meta name="robots" content="noindex, nofollow">` — hub is not for public indexing
- Styled with brand colors from config
- Load Google Fonts from `brand.fonts.google_fonts_url`
- Professional, client-facing design — not just an internal tool

### 3. Generate designs hub
**File**: `designs/index.html`

Client-facing design direction hub page.

Requirements:
- Title: `{client.name} — Design Directions`
- One card per entry in `designs[]` array, linking to `design-{key}/index.html`
- Each card shows: design name, mood description
- Styled with brand tokens
- Load Google Fonts from config
- Responsive grid layout

**File**: `designs/styles.css`

Shared styles for the designs hub:
- CSS custom properties from `brand.colors` mapped to `--{slug}-primary`, `--{slug}-accent`, etc.
- Typography from `brand.fonts`
- Responsive grid for design cards
- Hover effects on cards

### 4. Generate website/styles.css
**File**: `website/styles.css`

Base stylesheet with brand tokens as CSS custom properties:

```css
:root {
  --{slug}-primary: {colors.primary};
  --{slug}-accent: {colors.accent};
  --{slug}-accent-hover: {colors.accent_hover};
  --{slug}-bg: {colors.background};
  --{slug}-surface: {colors.surface};
  --{slug}-text: {colors.primary};
  --{slug}-light: #ffffff;
  --{slug}-font-heading: {fonts.heading};
  --{slug}-font-body: {fonts.body};
}
```

Plus base reset, typography, and utility classes using those variables.

### 5. Generate variant review hub
**File**: `website/variants/index.html`

Client-facing layout preference review page.

Requirements:
- Title: `{client.name} — Layout Review`
- For EACH page in `pages[]`, generate a review card with:
  - Page title as heading
  - 3 layout options (A / B / C) as radio buttons or visual selectors
  - Links to view each layout: Layout A = `../../website/{slug}.html`, Layout B = `{slug}-b.html`, Layout C = `{slug}-c.html`
  - Notes textarea for that page
- Summary panel showing all selections
- Form submission via Formsubmit.co AJAX:
  - POST to `https://formsubmit.co/ajax/{agency.primary_email}`
  - CC: `{agency.cc_email}`
  - Subject: `{client.name} — Layout Preferences`
  - Inline success message (no redirect)
- localStorage auto-save for all selections and notes
- Copy-to-clipboard fallback button
- Page count: `{pages.length}` pages, displayed in the heading
- Styled with brand tokens
- Load Google Fonts from config

### 6. Generate thank-you page
**File**: `website/variants/thank-you.html`

Confirmation page shown if a user navigates directly after form submission.

Requirements:
- Title: `{client.name} — Thank You`
- Heading: "Thank you for your feedback!"
- Message: "Your layout preferences have been submitted. We'll be in touch soon."
- Link back to review hub: `index.html`
- Link to project hub: `../../index.html`
- Styled with brand tokens
- Load Google Fonts from config
- Simple, clean layout

### 7. Generate CLAUDE.md
**File**: `CLAUDE.md`

Project context file for Claude Code sessions. Include:
- Project overview with client name, tagline
- GitHub Pages URL: `https://{agency.github_org}.github.io/{client.slug}/`
- Repo structure tree (reflecting generated files + empty directories)
- Brand tokens table
- CSS custom properties block
- Font loading HTML snippet
- Subdirectory descriptions (website, designs, landing, docs, variants)
- Deployment notes (GitHub Pages from main branch)
- Architecture notes (breakpoints, image strategy, no JS except review hub)
- All available commands: `/scaffold`, `/seo-report`, `/social-audit`, `/creative-process`, `/landing-page`
- `project.json` schema reference (all sections including `client.social` and `deliverables`)

### 8. Generate client asset brief
**File**: `docs/client-asset-brief.md`

Template document requesting assets from the client:
- Logo files (SVG, PNG, favicon)
- Brand photography / headshots
- Copywriting / service descriptions
- Testimonials / reviews
- Social media links
- Google Business Profile access
- Any existing brand guidelines
- Formatted as a checklist the client can work through

### 9. Generate project status tracker
**File**: `docs/project-status.md`

Deliverables checklist tracking progress through all project phases:

```markdown
# {client.name} — Project Status

## Phase: Setup
- [x] Scaffold generated
- [ ] Client asset brief sent
- [ ] GitHub Pages enabled

## Phase: Research
- [ ] Design inspiration research (references.md)
- [ ] SEO competitor report
- [ ] Social media audit

## Phase: Design
- [ ] Design direction A
- [ ] Design direction B
- [ ] Design direction C
- [ ] Creative process page
- [ ] Client review — direction chosen

## Phase: Build
- [ ] Website pages (Layout A)
- [ ] Layout B variants
- [ ] Layout C variants
- [ ] Landing page
- [ ] Client review — layout preferences received

## Phase: Handoff
- [ ] Preferences applied
- [ ] Final review
- [ ] Delivered
```

Conditionally adjust items based on `deliverables` flags:
- If `deliverables.social_audit` is `false`, omit "Social media audit" line
- If `deliverables.landing_page` is `false`, omit "Landing page" line
- If `deliverables.creative_process` is `false`, omit "Creative process page" line

## Important
- ALL values come from project.json — zero hardcoded client data
- Use the `client.slug` for CSS variable prefixes (e.g., `--marr-primary` for slug "marr")
- Maintain the same code quality and patterns as the MARR Laser project
- All HTML pages must be self-contained (inline styles or linked CSS, Google Fonts loaded)
- Review hub must use AJAX form submission (not redirect) — match the MARR Laser implementation
- Generated files should be production-quality, not scaffolding stubs
- The project hub (index.html) is now client-facing with 4-card layout and Foxhue branding
- Project status tracker adjusts based on deliverables flags
