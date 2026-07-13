# Discovery Synthesis

A Claude Code skill that turns raw discovery inputs — interview notes, call transcripts, support tickets — into structured insight cards. One card per problem found, ordered by confidence.

## How to use

Drop any number of input files into `input/`, then run:

```
/map-insights
```

All input types are treated equally. The skill reads everything, identifies distinct problems across sources, groups related signals, and asks for approval before generating cards.

## Input files

Drop files into `input/` — free-form notes, transcripts, support tickets, or any text format. No specific structure required.

## Output

- `output/synthesis_YYYY-MM-DD.md` — all insight cards in one file, ordered by confidence

## Insight card structure

Each card contains:

- **Problem statement** — one specific sentence written from the user's reality, not the product's feature gap
- **Evidence** — direct quotes where possible, with source file attribution
- **Confidence** — High, Medium, or Low
- **Why** — one sentence explaining the confidence rating
- **Recommended next step** — specific and actionable

## Confidence definitions

| Level | Criteria |
|---|---|
| **High** | Raised by multiple independent sources, or described with specific frequency or business impact, or observed as actual behaviour (not just stated preference) |
| **Medium** | Raised by one source with meaningful context, or by multiple sources but described vaguely |
| **Low** | Mentioned once, anecdotally, without specificity, frequency, or impact |
