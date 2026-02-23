# /brand-palette — Generate colour palette proposals for design directions

Read `project.json` for client context, research the client's industry and competitors via one Perplexity query, then generate a branded HTML report with 3 distinct palette proposals — one tailored to each design direction's mood.

## Steps

### 1. Read project.json
Extract:
- `client.name`, `client.industry`, `client.location`, `client.website` — research context
- `client.slug` — CSS variable prefix and file naming
- `brand.mood` (optional) — if present, use as a steering hint for colour selection
- `brand.fonts` — for "in use" preview cards
- `designs[]` — direction names and moods, to tailor each palette
- `agency.name` — report attribution
- `agency.primary_email`, `agency.cc_email` — feedback form submission targets

### 2. Research client and competitors via Perplexity

Use `mcp__perplexity__perplexity_search` — **one query** combining client site and competitor landscape:

**Query**: `"{client.website} colour scheme AND top {client.industry} websites {client.location} colour palettes brand identity"`

Collect:
- Colours currently used on the client's site (if any)
- Dominant colours used by competitors in this industry/location
- Common industry colour associations and patterns
- Differentiation opportunities — gaps in what competitors use

If `client.website` is empty or a placeholder, adjust the query to focus on industry competitors only.

### 3. Derive palettes from direction moods

For each entry in `designs[]`, map its `mood` description to appropriate colours using colour theory:

- Read the direction's `mood` string (e.g., "Clean, architectural white space with bold typography — confidence through restraint")
- Apply colour psychology: which hues, saturation levels, and value ranges match this mood?
- If `brand.mood` exists and has keywords, use them as an additional steering hint (not a hard constraint)
- Cross-reference with the Perplexity research to stay within the industry's range while differentiating

Each palette must include 5 colours:
- **primary** — main brand colour, used for text and key UI elements
- **accent** — secondary colour for buttons, links, highlights
- **accent_hover** — darker/lighter variant of accent for interactive states
- **background** — page background colour
- **surface** — card/section background colour (subtle contrast from background)

Palette generation rules:
- Each palette should feel distinct — different colour families, not just shade variations
- Each palette should align with its direction's mood description
- Avoid replicating competitor palettes exactly — differentiate while staying in the industry's range
- Ensure sufficient contrast for readability (primary text on background must be >= 4.5:1 WCAG AA)
- Accent colours should be vibrant enough for buttons but not overwhelming

### 4. Calculate WCAG contrast ratios

For each palette, calculate contrast ratios for these pairings:
- Primary text on background
- Primary text on surface
- White text on accent (button text)
- Primary text on accent (alternative button text)

Use the WCAG relative luminance formula:
- Relative luminance: `L = 0.2126 * R + 0.7152 * G + 0.0722 * B` (where R, G, B are linearised sRGB values)
- Contrast ratio: `(L1 + 0.05) / (L2 + 0.05)` where L1 is the lighter colour

Rate each pairing:
- >= 7:1 -> AAA (excellent)
- >= 4.5:1 -> AA (good)
- >= 3:1 -> AA Large (acceptable for large text only)
- < 3:1 -> Fail (needs adjustment)

If any critical pairing (text on background) fails AA, adjust the palette before finalising.

### 5. Generate report

**File**: `docs/brand-palette.html`

A self-contained HTML report. Requirements:

**Styling**:
- Brand-neutral design — the client doesn't have brand colours yet
- Use a clean grey/white palette: `#1a1a1a` text, `#ffffff` background, `#f5f5f5` surface, `#e0e0e0` borders
- System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
- Load Google Fonts from `brand.fonts.google_fonts_url` — needed for "in use" preview cards
- Professional, client-ready design
- Print-friendly CSS
- Responsive layout
- Page wrapper max-width: `1100px`

**Report sections**:

1. **Header**
   - Title: `{client.name} — Brand Palette Exploration`
   - Date generated
   - Prepared by: `{agency.name}`

2. **Direction Overview**
   - List the 3 design directions with their mood descriptions
   - If `brand.mood` has keywords, display them as styled tags/chips with a note: "These mood keywords are used as a steering hint across all palettes"
   - If `brand.mood` is empty/absent, skip the mood tags — direction moods are sufficient

3. **Industry & Competitor Context**
   - What Perplexity found about colour usage in `{client.industry}`
   - Current client site colours (if found)
   - Common competitor colour patterns
   - Key colour associations for this sector
   - Note: attribute insights to research, not invention

4. **Palette Proposals** (one section per design direction)

   For each direction in `designs[]`:

   **a. Direction header**
   - Direction name (e.g., "Direction A — Studio Minimal")
   - Mood description from `designs[].mood`

   **b. Colour swatches**
   - 5 rectangular swatches in a horizontal row
   - Each swatch shows the colour fill with hex value overlaid (white or dark text for contrast)
   - Labels below: Primary, Accent, Accent Hover, Background, Surface
   - Swatches should be at least 100px wide, 80px tall

   **c. "In use" preview card**
   - A mini card (max 400px wide) demonstrating the palette in context:
     - Card background uses the `surface` colour
     - Heading text in the `heading` font from `brand.fonts`, coloured with `primary`
     - Body text in the `body` font from `brand.fonts`, coloured with `primary`
     - A button using `accent` background with white text
     - Card sits on the `background` colour
   - This gives the client an instant feel for how the palette works

   **d. WCAG contrast table**
   - Table with columns: Pairing | Ratio | Rating
   - Rows: text on background, text on surface, white on accent, text on accent
   - Rating uses colour-coded badges: green for AAA, amber for AA, red for Fail
   - Visual indicators, not just text

   **e. Rationale**
   - 2-3 sentences explaining why this palette suits this direction
   - Reference the direction's mood and research findings
   - Note how it differentiates from competitors

   **f. Font pairing note**
   - Brief note on how the palette complements the configured fonts
   - E.g., "The warm taupe accent pairs naturally with Cormorant Garamond's refined serifs"

5. **Competitor Colour Audit**
   - Table with columns: Competitor | Primary | Accent | Background | Notes
   - 3-5 rows from the Perplexity research
   - Show colour swatches (small inline squares) alongside hex values where available
   - Note patterns and gaps — where can the client differentiate?

6. **Your Feedback** (client-facing feedback form)
   - Section title: "Your Feedback"
   - Intro text: "Which palette direction feels right for {client.name}? Pick your preference below and add any notes — we'll refine from there."
   - **Three radio-style palette cards** (one per direction from `designs[]`):
     - Each card shows: direction label (A/B/C), direction name, and a mini row of 5 colour swatches (30px squares using that palette's hex values)
     - Cards are selectable via hidden radio input — selected card gets a dark border highlight
     - Grid layout: 3 columns on desktop, 1 column on mobile
   - **Notes textarea**: label "Any colour notes?", placeholder "e.g. 'Love the copper but want a lighter background'"
   - **Client name** field (required) + **email** field (optional)
   - **Submit button** + **Copy to Clipboard** fallback button
   - **Form configuration**:
     - `action="https://formsubmit.co/{agency.primary_email}"` (from `project.json`)
     - Hidden field `_cc` = `{agency.cc_email}` (from `project.json`)
     - Hidden field `_subject` = `"{client.name} — Palette Preference"`
     - Hidden field `_captcha` = `"false"`
   - **AJAX submission**: `fetch` with `Accept: application/json` header, inline success message (no redirect)
   - **localStorage auto-save**: persist palette selection + notes + name/email across page reloads (key: `{client.slug}_palette_feedback`)
   - **Copy to clipboard**: compile plaintext summary of selection and notes, with `navigator.clipboard` and `execCommand('copy')` fallback
   - **Inline `<script>`** at end of `<body>` handles all form logic (no external JS)
   - **Print CSS**: hide the form section in print media query
   - Note: these are starting points — palettes can be refined based on client feedback

**Navigation**: Include a subtle "<- Project Hub" link at the top of the page, linking to `../index.html`. Style it as a small, muted link above the header — not visually dominant but always accessible. Grey text, darker on hover.

**Footer**: `Prepared by {agency.name} | Confidential`

## Important
- Colour research must come from a real Perplexity query, not invented
- If Perplexity returns limited data, note it honestly and supplement with general colour theory knowledge
- Report must be client-ready — professional language, clear visual presentation
- HTML must be fully self-contained (style block, Google Fonts via link)
- Include print media queries for clean printing
- WCAG contrast calculations must be accurate — use the actual luminance formula, not estimates
- The "in use" preview cards are critical — clients need to see colours in context, not just swatches
- Palette proposals should feel genuinely distinct, not just hue-shifted versions of each other
- All client values from project.json — no hardcoded names or data
