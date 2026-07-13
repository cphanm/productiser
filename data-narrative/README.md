# Data Narrative

A Claude Code skill that turns raw analytics data into a stakeholder narrative structured around a clear recommendation — one document, five team audiences.

Based on Rich Mironov's [Money Stories](https://www.mironov.com/book/) framework: every narrative anchors on financial consequence, not just data observation. The recommendation leads; the team sections are the evidence.

## How to use

Drop any number of data files into `input/data/`, then run:

```
/tell-the-story
```

Accepted formats: `.csv`, `.tsv`, `.xlsx`, `.xls`. Files from different tools don't need to share a common structure — the skill figures out what each file measures and which teams it's relevant to. One file can inform multiple team sections.

The skill asks three context questions before generating:
1. What question does this data answer, and what's the business context?
2. What do you want stakeholders to do or decide?
3. What is the financial consequence if this data is ignored?

## Output

- `output/narrative_YYYY-MM-DD.md` — the stakeholder narrative

## Narrative structure

- **Recommendation** — the decision or action, stated first, anchored in financial consequence
- **For Leadership** — business and financial argument
- **For Sales** — pipeline and revenue argument
- **For Customer Support** — customer experience and volume argument
- **For Engineering** — product performance and technical argument
- **For Operations** — capacity and process argument

Each section leads with the action, follows with the evidence, and cites its source file(s). Sections with no relevant data are omitted rather than fabricated.
