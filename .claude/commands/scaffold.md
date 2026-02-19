# /scaffold — Generate project infrastructure from project.json

Read `project.json` from the repo root and generate all scaffolded files. Every generated file must pull its values (names, colors, fonts, emails, pages, designs) from the config — never hardcode client-specific data.

## Steps

### 1. Read project.json
Parse the config file at the repo root. Extract:
- `agency.*` — agency name, emails, GitHub org
- `client.*` — client name, short name, tagline, slug, industry, location, phone, email
- `brand.colors.*` — primary, accent, accent_hover, background, surface
- `brand.fonts.*` — heading, body, google_fonts_url
- `designs[]` — array of { key, name, mood }
- `pages[]` — array of { key, title, slug }

### 2. Generate root index.html (project hub)
**File**: `index.html`

A simple internal hub page linking to all project deliverables. Not client-facing.

Requirements:
- Title: `{client.name} — Project Hub`
- Links to: `designs/index.html`, `website/index.html`, `website/variants/index.html`, `docs/`
- Styled with brand colors from config
- Load Google Fonts from `brand.fonts.google_fonts_url`
- Professional but minimal — this is an internal tool

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

### 6. Generate CLAUDE.md
**File**: `CLAUDE.md`

Project context file for Claude Code sessions. Include:
- Project overview with client name, tagline
- Repo structure tree (reflecting generated files + empty directories)
- Brand tokens table
- CSS custom properties block
- Font loading HTML snippet
- Subdirectory descriptions (website, designs, landing, docs, variants)
- Deployment notes (GitHub Pages from main branch)
- Architecture notes (breakpoints, image strategy, no JS except review hub)
- GitHub Pages URL: `https://{agency.github_org}.github.io/{client.slug}/`

### 7. Generate client asset brief
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

## Important
- ALL values come from project.json — zero hardcoded client data
- Use the `client.slug` for CSS variable prefixes (e.g., `--marr-primary` for slug "marr")
- Maintain the same code quality and patterns as the MARR Laser project
- All HTML pages must be self-contained (inline styles or linked CSS, Google Fonts loaded)
- Review hub must use AJAX form submission (not redirect) — match the MARR Laser implementation
- Generated files should be production-quality, not scaffolding stubs
