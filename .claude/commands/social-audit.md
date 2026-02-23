# /social-audit — Generate social media audit and strategy report

Read `project.json` for client context, then use Perplexity MCP to research the client's social landscape and generate a branded report.

## Steps

### 1. Read project.json
Extract:
- `client.name`, `client.industry`, `client.location` — for search context
- `client.slug` — for CSS variable prefixes
- `client.social.*` — existing social profile URLs (instagram, facebook, tiktok, linkedin, google_business)
- `brand.colors.*`, `brand.fonts.*` — for report styling
- `agency.name` — for report attribution

### 2. Audit current social presence via Perplexity

If `client.social` URLs are provided, use `mcp__perplexity__perplexity_search` to assess each active profile:

**For each non-empty social URL**:
- Search: `"{client.name}" site:{platform}.com`
- Assess: posting frequency, content types, engagement levels, follower count (if visible), profile completeness
- Note: last post date, bio quality, link in bio, highlights/pinned content

If no social URLs provided, search for the client's presence:
- `"{client.name}" {client.location} social media`
- Note what platforms they appear on organically

### 3. Competitor social analysis via Perplexity

Use `mcp__perplexity__perplexity_research` to investigate competitor social strategies:

**Search queries** (adapt based on industry):
1. `"best {client.industry} social media accounts {client.location}"`
2. `"{client.industry} businesses {client.location} instagram facebook"`
3. `"successful {client.industry} social media strategy examples"`

**For each competitor found, assess**:
- Which platforms they're active on
- Posting frequency and content types
- Engagement patterns (comments, shares, saves)
- Content themes that perform well
- Visual style and branding consistency
- Use of stories, reels, video content

### 4. Platform analysis via Perplexity

Use `mcp__perplexity__perplexity_search` to get current platform insights:
- `"{client.industry} best social media platforms 2025 2026"`
- `"social media trends {client.industry}"`

Determine which platforms matter most for this industry and location.

### 5. Generate report

**File**: `docs/social-media-audit.html`

A self-contained, branded HTML report. Requirements:

**Styling**:
- Use brand colors from `project.json` (CSS custom properties with `--{slug}-` prefix)
- Load Google Fonts from `brand.fonts.google_fonts_url`
- Professional, client-ready design matching the SEO report aesthetic
- Print-friendly CSS
- Responsive layout

**Report sections**:

1. **Cover / Header**
   - Report title: `{client.name} — Social Media Audit`
   - Date generated
   - Prepared by: `{agency.name}`

2. **Executive Summary**
   - 3-5 bullet overview of key findings
   - Overall social media maturity assessment
   - Top 3 priority actions

3. **Current Social Presence**
   - Assessment of each platform the client is on
   - Profile completeness scores (visual indicators)
   - Posting frequency and consistency
   - Content quality observations
   - Engagement levels
   - If client has no social presence, frame as opportunity rather than criticism

4. **Competitor Social Analysis**
   - One profile per competitor found
   - Platforms used, posting frequency, content themes
   - What's working well for them (engagement drivers)
   - Opportunities they're missing
   - Visual comparison table: client vs competitors across platforms

5. **Platform Recommendations**
   - Ranked list of recommended platforms for this industry
   - For each platform: why it matters, expected audience, content format
   - Prioritise: which platform to focus on first vs later
   - Include platform-specific tips (e.g., Instagram Reels frequency, Facebook group strategy)

6. **Content Strategy**
   - Recommended content pillars (3-5 themes)
   - Post types that work in this industry (educational, behind-the-scenes, testimonials, etc.)
   - Posting frequency recommendations per platform
   - Content calendar framework (weekly themes)
   - Visual style recommendations

7. **Quick Wins**
   - Profile optimisation checklist (bio, profile photo, link, highlights)
   - Google Business Profile optimisation
   - Cross-platform linking
   - Review response strategy
   - Hashtag strategy

8. **Growth Roadmap**
   - Month 1: Foundation (profile setup, content pillars, initial posts)
   - Month 2: Consistency (regular posting schedule, engagement habits)
   - Month 3: Growth (paid promotion, collaborations, community building)
   - KPIs to track per month

9. **Next Steps**
   - Prioritised action items
   - Tools and resources recommended
   - Suggested timeline

**Navigation**: Include a subtle "← Project Hub" link at the top of the page, linking to `../index.html`. Style it as a small, muted link above the header — not visually dominant but always accessible. Use the accent colour on hover.

**Footer**: `Prepared by {agency.name} | Confidential`

## Important
- All data must come from real Perplexity research, not invented
- If Perplexity returns limited data, note it honestly — "Limited public data available"
- Report must be client-ready — professional language, no jargon without explanation
- Frame gaps as opportunities, not failures — encouraging tone
- HTML must be fully self-contained (style block + Google Fonts link)
- Include print media queries for clean printing
- All brand values from project.json, no hardcoded colors/fonts
- Match the same report quality and structure as /seo-report
