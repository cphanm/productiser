# Competitive Analysis Toolkit

A Claude Code skill that runs a structured competitive analysis for any product, generates a full report, and tracks changes month-over-month using versioned snapshots.

---

## How it works

The toolkit is a single Claude Code slash command: `/analyse-competitors`

Each run:
1. Reads your product description and seed competitor list from `original/`
2. Searches the web to discover new competitors
3. Asks for approval before doing deep research
4. Researches every competitor — features, pricing, App Store ratings, positioning, messaging, and sales channels
5. Compares findings against the previous month's snapshot and generates a "What Changed" section
6. Writes a full competitive analysis report to `output/competitive_analysis.md`
7. Saves a structured snapshot to `output/snapshot_YYYY-MM.json` for next month's diff

---

## Report structure

Each report contains:

1. **Executive Summary** — top strategic takeaways
2. **Competitor Profiles** — features, audience, pricing, strengths, weaknesses, positioning, and distribution per competitor
3. **Competitive Landscape Map** — feature comparison table
4. **Positioning & Messaging Map** — taglines, emotional hooks, brand voice, open messaging angles
5. **Sales & Distribution Overview** — platforms, geographies, acquisition channels, B2B deals
6. **Strategic Gaps & Opportunities** — whitespace and defensible edges for the product
7. **Recommendations** — specific, prioritised, opinionated

When a previous snapshot exists, a **What Changed This Month** section is prepended — price changes, rating shifts, new entrants, status changes, positioning pivots, and new distribution moves.

---

## Setup

### 1. Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

### 2. Clone this repo

```bash
git clone https://github.com/your-username/productiser.git
cd productiser
```

### 3. Create your input files

These files are gitignored — they stay local and are never committed.

**`original/product.md`** — describe your product:

```markdown
# Product Name

One-paragraph description of what the product does and who it's for.

## Product categories
- Category 1 (e.g. Women's health app)
- Category 2 (e.g. Keto tracking app)

## Target audience
Who the product serves. Include age range, life stage, or any other relevant segment.

## Market
- Primary market: [country or region]
- Languages supported: [language(s)]

## Platform
- iOS / Android / Web (note which is primary and which is planned)

## Stage
[Beta / Launched / Pre-launch] — brief note on current status.

## Business model
[Subscription / Freemium / One-time purchase] — note pricing model and price if known.

## Core features
- Feature 1
- Feature 2
- Feature 3

## Key differentiators
- What makes this product distinct from every competitor
- Be specific — vague differentiators produce vague analysis

## Out of scope
- Things the product explicitly does not do
- Helps the skill frame strengths and weaknesses accurately
```

**`original/competitors.md`** — seed competitor list with known data:

```markdown
## Competitor Name
url: https://competitor.com
app_store: https://apps.apple.com/app/...
product_hunt: https://www.producthunt.com/products/...
pricing: Free + Premium $X/year
notes: One line on why this competitor matters
```

Add as many or as few as you know. The skill discovers additional competitors automatically.

### 4. Run the analysis

```bash
claude
```

Then type:

```
/analyse-competitors
```

To include specific competitors not on the seed list:

```
/analyse-competitors Noom, WeightWatchers, Ozempic Companion
```

---

## Monthly workflow

Run once a month. The skill handles the rest:

- Detects the previous snapshot automatically
- Diffs pricing, ratings, positioning, and distribution changes
- Flags new entrants and defunct competitors
- Prepends a "What Changed" section to the report

No manual comparison needed.

---

## Project structure

```
productiser/
├── .claude/
│   └── commands/
│       └── analyse-competitors.md   # The skill definition
├── original/                        # gitignored — your private inputs
│   ├── product.md                   # Product description
│   └── competitors.md               # Seed competitor list
├── output/                          # gitignored — generated reports
│   ├── competitive_analysis.md      # Latest full report
│   └── snapshot_YYYY-MM.json        # Monthly snapshots for diffing
├── .gitignore
└── README.md
```

`original/` and `output/` are excluded from version control. They contain your product details and competitive intelligence, which should stay private.

---

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

---

## Requirements

- [Claude Code](https://claude.ai/code) with web search enabled
- An `original/` directory with at least `product.md` and `competitors.md`
