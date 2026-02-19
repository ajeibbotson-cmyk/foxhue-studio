# /seo-report — Generate SEO competitor analysis report

Read `project.json` for client context, then use Perplexity MCP to research competitors and generate a branded SEO competitor report.

## Steps

### 1. Read project.json
Extract:
- `client.name`, `client.industry`, `client.location` — for competitor search context
- `client.slug` — for file naming and CSS prefixes
- `brand.colors`, `brand.fonts` — for report styling
- `agency.name` — for report attribution

### 2. Find competitors via Perplexity

Use `mcp__perplexity__perplexity_search` to find 3-5 direct competitors:

**Search queries** (run in sequence, adapt based on results):
1. `"best {client.industry} businesses in {client.location}"`
2. `"{client.industry} near {client.location} reviews"`
3. `"top {client.industry} companies {client.location} area"`

Collect: business names, websites, locations, brief descriptions.

### 3. Deep competitor analysis via Perplexity

For each competitor found, use `mcp__perplexity__perplexity_research` to investigate:

- **Online presence**: Website quality, social media activity, review platforms
- **SEO indicators**: Site structure, service pages, blog presence, local landing pages
- **Google Business Profile**: Completeness, review count, rating, response rate
- **Content strategy**: Blog frequency, topics covered, content quality
- **Keywords**: What terms they appear to target
- **Technical**: Mobile-friendliness, page speed observations, HTTPS
- **Local SEO**: Location pages, schema markup, directory listings

### 4. Gap analysis

Compare competitors against the client:
- What competitors do well that the client is missing
- Keyword opportunities the client isn't targeting
- Content gaps (topics competitors cover that the client doesn't)
- Technical improvements needed
- Local SEO opportunities

### 5. Generate report

**File**: `docs/seo-competitor-report.html`

A self-contained, branded HTML report. Requirements:

**Styling**:
- Use brand colors from `project.json`
- Load Google Fonts from config
- Professional, client-ready design
- Print-friendly CSS
- Responsive layout

**Report sections**:

1. **Cover / Header**
   - Report title: `{client.name} — SEO Competitor Analysis`
   - Date generated
   - Prepared by: `{agency.name}`

2. **Executive Summary**
   - 3-5 bullet overview of key findings
   - Overall competitive position assessment
   - Top 3 priority actions

3. **Competitor Profiles** (one per competitor)
   - Business name and location
   - Website URL
   - Services overview
   - Online presence score (1-5 visual indicator)
   - Key strengths
   - Key weaknesses

4. **SEO Gap Analysis Table**
   - Rows: SEO factors (site structure, blog, local SEO, reviews, speed, etc.)
   - Columns: Client + each competitor
   - Visual indicators: green/amber/red or checkmarks

5. **Keyword Opportunities**
   - Keywords competitors rank for that the client should target
   - Estimated difficulty/priority
   - Recommended pages to create or optimize

6. **Technical Recommendations**
   - Site speed improvements
   - Mobile optimization
   - Schema markup suggestions
   - HTTPS and security

7. **Content Strategy Recommendations**
   - Blog topic suggestions based on competitor analysis
   - Content frequency recommendations
   - Content types that work in the industry

8. **Local SEO Checklist**
   - Google Business Profile optimization steps
   - Directory listings to create/claim
   - Review generation strategy
   - Local schema markup

9. **Next Steps**
   - Prioritized action items
   - Quick wins vs long-term strategies
   - Suggested timeline

**Footer**: `Prepared by {agency.name} | Confidential`

## Important
- All data must come from real Perplexity research, not invented
- If Perplexity returns limited data for a competitor, note it honestly
- Report must be client-ready — professional language, no technical jargon without explanation
- HTML must be fully self-contained (inline CSS or style block, Google Fonts via link)
- Include print media queries for clean printing
- All brand values from project.json, no hardcoded colors/fonts
