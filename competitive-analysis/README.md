# Competitive Analysis

A Claude Code skill that runs a structured competitive analysis for any product. Discovers competitors, researches features, pricing, positioning, and distribution, then generates a full report. Tracks changes month-over-month using versioned snapshots.

## How to use

Drop your input files into `input/`, then run:

```
/analyse-competitors
```

To include specific competitors not on the seed list:

```
/analyse-competitors Noom, WeightWatchers, Ozempic Companion
```

## Input files

**`input/product.md`** — your product description:

```markdown
# Product Name

One-paragraph description of what the product does and who it's for.

## Product categories
## Target audience
## Market
## Platform
## Stage
## Business model
## Core features
## Key differentiators
## Out of scope
```

**`input/competitors.md`** — seed competitor list:

```markdown
## Competitor Name
url: https://competitor.com
app_store: https://apps.apple.com/app/...
product_hunt: https://www.producthunt.com/products/...
pricing: Free + Premium $X/year
notes: One line on why this competitor matters
```

The skill discovers additional competitors automatically. Add as many or as few as you know.

## Output

- `output/competitive_analysis.md` — full report, overwritten each run
- `output/snapshot_YYYY-MM.json` — versioned snapshot for month-over-month diffing

## Report structure

1. **Executive Summary** — top strategic takeaways
2. **Competitor Profiles** — features, audience, pricing, strengths, weaknesses, positioning, distribution
3. **Competitive Landscape Map** — feature comparison table
4. **Positioning & Messaging Map** — taglines, emotional hooks, brand voice, open angles
5. **Sales & Distribution Overview** — platforms, geographies, acquisition channels, B2B deals
6. **Strategic Gaps & Opportunities** — whitespace and defensible edges
7. **Recommendations** — specific, prioritised, opinionated

When a previous snapshot exists, a **What Changed This Month** section is prepended automatically.

## Monthly workflow

Run once a month. The skill detects the previous snapshot, diffs pricing, ratings, positioning, and distribution changes, flags new entrants and defunct competitors, and prepends the "What Changed" section. No manual comparison needed.

## Snapshot schema

Each `snapshot_YYYY-MM.json` captures per competitor:

| Field | Description |
|---|---|
| `name` | Competitor name |
| `url` | Primary URL |
| `status` | `active`, `defunct`, `acquired`, `rebranded`, or `unknown` |
| `pricing` | Exact pricing string as found during research |
| `app_store_rating` | Float or `null` |
| `target_audience` | One-line description |
| `key_features` | Array of feature strings |
| `positioning` | Hero tagline or value prop from their landing page |
| `messaging_themes` | Array: `fear`, `aspiration`, `authority`, `community`, `empowerment`, `science` |
| `platforms` | Array: `iOS`, `Android`, `Web`, `Apple Watch`, etc. |
| `geographies` | Array of key markets |
| `acquisition_channels` | Array of primary user acquisition methods |
| `b2b_deals` | Description of institutional partnerships, or empty string |
| `notable` | Funding, rebrands, major news this period |
