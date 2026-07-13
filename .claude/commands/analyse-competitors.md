You are running a competitive analysis. Follow these steps in order.

## Step 1 — Load context

Read both files:
- `competitive-analysis/input/product.md` — product description and positioning
- `competitive-analysis/input/competitors.md` — seed competitor list with URLs, pricing, and notes

From `original/product.md`, extract:
- PRODUCT_NAME — the name of the product
- PRODUCT_CATEGORY — the market or industry it operates in
- TARGET_AUDIENCE — who it serves
- KEY_DIFFERENTIATORS — what makes it distinct

Use these throughout all subsequent steps instead of any hardcoded product-specific references.

Then check for a previous snapshot:
- List files in `competitive-analysis/output/` matching the pattern `snapshot_*.json`
- If one or more exist, load the most recent one (highest YYYY-MM in filename)
- Store it as PREVIOUS_SNAPSHOT for use in Step 5

## Step 2 — Discover competitors

Search the web to find additional competitors not already in `original/competitors.md`. Derive search categories from PRODUCT_CATEGORY, TARGET_AUDIENCE, and KEY_DIFFERENTIATORS — do not use hardcoded categories.

Merge discovered competitors with the seed list. Remove duplicates.

If $ARGUMENTS is not empty, treat it as a comma-separated list of additional competitors to include.

## Step 3 — Approval gate: competitor list

Present the full merged competitor list to the user and ask:
- Are there any competitors to remove?
- Are there any to add?

Wait for the user's response before proceeding. Update the list based on feedback.

## Step 4 — Deep research

For each approved competitor, search the web to gather:
- Core features and value proposition
- Target audience
- Pricing (use the `pricing` field from competitors.md as a starting point — verify and update if outdated)
- Strengths and weaknesses relative to PRODUCT_NAME
- Recent updates, news, or funding
- App Store ratings and notable user review themes
- Product Hunt feedback if available

**Positioning & Messaging** — gather for each competitor:
- Hero tagline or headline on their landing page (exact wording if possible)
- Core value proposition in one sentence (what problem they claim to solve, for whom)
- Key emotional themes in their marketing: fear, aspiration, authority, community, empowerment, science
- How they differentiate themselves verbally from competitors
- Brand voice: clinical, warm and supportive, science-forward, coach-like, community-driven

**Sales & Distribution** — gather for each competitor:
- Platforms available: iOS, Android, web app, wearables, etc.
- Languages and geographic markets
- Primary user acquisition channels: organic App Store / SEO, paid social, influencer / creator partnerships, press coverage, medical or clinical referrals, employer health plans, insurance partnerships, healthcare system deals, App Store featuring
- B2B angle: any employer, insurer, or healthcare system distribution deals
- Notable partnerships that drive distribution

Use the `app_store`, `product_hunt`, and `url` fields from `competitive-analysis/input/competitors.md` as your starting URLs.

## Step 5 — Synthesise report

### 5a — Build the diff (only if PREVIOUS_SNAPSHOT exists)

Compare each competitor in the new data against PREVIOUS_SNAPSHOT. Flag:
- **Price changes** — any pricing field that differs from the previous value
- **Rating changes** — App Store rating moved by ±0.1 or more
- **Status changes** — competitor went defunct, was acquired, rebranded, or relaunched
- **New competitors** — present in new list but absent from PREVIOUS_SNAPSHOT
- **Dropped competitors** — present in PREVIOUS_SNAPSHOT but removed from the approved list this run (with reason)
- **Notable updates** — new features, funding rounds, major news discovered during research
- **Positioning changes** — tagline or core value proposition changed (signals a strategic pivot worth flagging)
- **Distribution changes** — new platform, new geography, new B2B channel, or new notable partnership

### 5b — Write the report

Write a competitive analysis report in markdown with this structure:

**If PREVIOUS_SNAPSHOT exists**, prepend a "What Changed This Month" section before the Executive Summary:

```
## What Changed This Month (vs [YYYY-MM])

### New competitors
- [name] — [one-line description]

### Price changes
- [name]: [old price] → [new price]

### Rating changes
- [name]: [old rating] → [new rating]

### Status changes
- [name]: [what changed]

### Notable updates
- [name]: [what happened]

### Positioning changes
- [name]: [old tagline] → [new tagline]

### Distribution changes
- [name]: [what changed — new platform, geography, B2B deal, etc.]

### No changes detected
- [name], [name], [name] — unchanged
```

Then the standard report sections:

1. **Executive Summary** — 3-5 bullet points on the most important strategic takeaways for PRODUCT_NAME
2. **Competitor Profiles** — one section per competitor. Each profile must include:
   - Features, target audience, pricing, strengths, weaknesses
   - **Positioning & Messaging** — tagline, value prop, emotional themes, brand voice
   - **Sales & Distribution** — platforms, geographies, acquisition channels, B2B deals
3. **Competitive Landscape Map** — feature comparison table (rows = features, columns = competitors)
4. **Positioning & Messaging Map** — table comparing how each competitor positions itself:
   - Columns: Competitor | Core tagline | Primary emotional hook | Brand voice | Who they say they're for
   - Call out which positioning angles are overcrowded and which are open for PRODUCT_NAME
5. **Sales & Distribution Overview** — table comparing go-to-market approach:
   - Columns: Competitor | Platforms | Key geographies | Primary acquisition channel | B2B / institutional deals
   - Highlight distribution advantages PRODUCT_NAME can exploit
6. **Strategic Gaps & Opportunities** — where PRODUCT_NAME has whitespace or a defensible edge
7. **Recommendations for PRODUCT_NAME** — specific, actionable, prioritised

Be direct and opinionated. Call out what PRODUCT_NAME should copy, what to avoid, and what no one else is doing yet.

## Step 6 — Save report and snapshot

### 6a — Save the report
Save the report to `competitive-analysis/output/competitive_analysis.md`, overwriting any previous version.

### 6b — Save the snapshot
Write a snapshot file to `competitive-analysis/output/snapshot_YYYY-MM.json` (use the current year and month).

The snapshot must follow this exact schema — one object per competitor:

```json
{
  "generated": "YYYY-MM",
  "date": "YYYY-MM-DD",
  "competitor_count": 0,
  "competitors": [
    {
      "name": "Competitor Name",
      "url": "https://...",
      "status": "active",
      "pricing": "exact pricing string as found during research",
      "app_store_rating": 4.7,
      "target_audience": "one-line description",
      "key_features": ["feature 1", "feature 2", "feature 3"],
      "positioning": "exact hero tagline or value prop from their landing page",
      "messaging_themes": ["aspiration", "authority"],
      "platforms": ["iOS", "Android", "Web"],
      "geographies": ["US", "UK", "EU"],
      "acquisition_channels": ["organic App Store", "paid social", "press"],
      "b2b_deals": "description of any employer/insurer/healthcare partnerships, or empty string if none",
      "notable": "any funding, rebrands, major news, or blank string if none"
    }
  ]
}
```

`status` must be one of: `active`, `defunct`, `acquired`, `rebranded`, `unknown`.  
`app_store_rating` must be a number (float) or `null` if not found.  
`messaging_themes` must use values from: `fear`, `aspiration`, `authority`, `community`, `empowerment`, `science`.  
`acquisition_channels` must use values from: `organic App Store`, `organic SEO`, `paid social`, `influencer`, `press`, `medical referral`, `employer health plan`, `insurance`, `healthcare system`, `App Store featuring`.  
Do not omit any competitor from the snapshot even if data is sparse — use `"unknown"` for missing string fields and `[]` for missing arrays.

### 6c — Confirm to the user
Confirm both file paths and ask whether to proceed to the next stage.
