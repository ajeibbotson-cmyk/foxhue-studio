# /creative-process — Generate client-facing creative process page

Read `project.json` and `inspiration/references.md`, then generate a branded HTML page walking clients through how design directions were developed.

**Run this after** `inspiration/references.md` is populated (during the design research phase, not at scaffold time).

## Steps

### 1. Read project.json
Extract:
- `client.name`, `client.short_name`, `client.tagline` — for page headings
- `client.slug` — for CSS variable prefixes and file naming
- `client.industry` — for contextual copy
- `brand.mood` — for colour exploration section (mood keywords as chips)
- `brand.colors.*` — for page styling and colour swatch display
- `brand.fonts.*` — for typography
- `designs[]` — for design direction summary cards (including `palette` if available)
- `agency.name` — for footer attribution

### 2. Read inspiration/references.md
Parse the references file for:
- Site names, URLs, and descriptions
- Categories and "what's good" notes
- "Borrow for this project" notes
- Group references by which design direction they inspired (if tagged), otherwise present as a general inspiration gallery

### 3. Generate creative process page

**File**: `docs/creative-process.html`

A self-contained, branded HTML page. Requirements:

**Styling**:
- Use brand colors from `project.json` (CSS custom properties with `--{slug}-` prefix)
- Load Google Fonts from `brand.fonts.google_fonts_url`
- Professional, client-ready design matching the designs hub aesthetic
- Print-friendly CSS
- Responsive layout (breakpoints: 992px, 767px, 477px)
- Elegant typography with heading font for titles, body font for content

**Page sections**:

1. **Cover Hero**
   - Client name (large heading)
   - "Our Creative Process" subtitle
   - Brief intro: "How we developed your design directions"
   - Styled with brand primary/accent colors

2. **The Brief**
   - Summary of client goals and requirements
   - Pull from `client.tagline`, `client.industry`, `client.location`
   - Frame as: "You came to us with a vision for [industry context]. Here's how we brought it to life."
   - Keep this section brief — the client knows their own brief

3. **Research Phase**
   - Methodology overview: "We researched [number] websites across [industries/categories] to identify patterns, trends, and opportunities."
   - Describe the research approach: industry benchmarking, aesthetic exploration, UX pattern analysis
   - Keep professional but accessible — this is for clients, not designers

4. **Inspiration Gallery**
   - Display each reference from `references.md` as a card
   - Show: site name (linked), category, key takeaway
   - If references are tagged to specific design directions, group them under direction headings
   - If not tagged, present as a unified gallery with category labels
   - Visual grid layout, responsive

5. **Design Principles**
   - Extract common themes from the research notes
   - Present 3-5 principles that emerged (e.g., "Warm approachability", "Clean hierarchy", "Bold typography")
   - Each principle gets a short heading and 1-2 sentence explanation
   - These should feel like they naturally bridge from research to design directions

6. **Colour Exploration** (conditional)

   Check if `docs/brand-palette.html` exists in the project.

   **If it exists** (palette was generated via `/brand-palette`):
   - Section heading: "Finding Your Colours"
   - Brief narrative: explain that colours were researched, not picked arbitrarily
   - Display the `brand.mood` keywords as styled tags/chips
   - Summarise the research approach: industry colour norms, colour psychology for the mood keywords, competitor colour audit
   - For each design direction, show a mini swatch row (5 colours from the palette) with the direction name — giving the client a visual taste before the full directions
   - Link to the full palette report: `brand-palette.html` ("View the complete palette exploration")
   - Tone: "We explored colour palettes grounded in research — industry conventions, colour psychology, and competitive differentiation."

   **If it doesn't exist** (client had a style guide):
   - Section heading: "Brand Colours"
   - Brief note: "Your brand colours were drawn from your existing brand guidelines, ensuring consistency across all touchpoints."
   - Show the 5 brand colours from `brand.colors` as a single swatch row
   - Keep this section short — the client already knows their own colours

7. **Three Directions**
   - One summary card per entry in `designs[]`
   - Each card shows: direction name, mood description
   - Link each card to `../designs/design-{key}/index.html`
   - Brief framing text: "Based on our research, we developed three distinct approaches for your consideration."
   - Cards should be visually prominent — this is the payoff of the page

8. **Footer**
   - `Prepared by {agency.name}`
   - Professional, minimal
   - Matching brand styling

**Navigation**: Include a subtle "← Project Hub" link at the top of the page, linking to `../index.html`. Style it as a small, muted link above the hero — not visually dominant but always accessible. Use the accent colour on hover.

## Important
- ALL values come from `project.json` and `references.md` — no hardcoded client data
- The page should tell a coherent narrative: Brief → Research → Insights → Colours → Directions
- Write copy that's professional but warm — this is client-facing, not a design spec
- If `references.md` has few entries, adapt the gallery section gracefully (don't show an empty grid)
- HTML must be fully self-contained (style block + Google Fonts link)
- Match the code quality and patterns from the SEO report and designs hub
