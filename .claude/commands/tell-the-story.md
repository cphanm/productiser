You are turning raw data into a stakeholder narrative. Follow these steps in order.

## Step 1 — Load all data files

Check for files in `data-narrative/input/data/`. If the directory is empty or does not exist, ask the PM to drop their data files there and re-run.

Read every file found. For each file, handle based on extension:
- `.csv` or `.tsv` — read directly with the Read tool
- `.xlsx` or `.xls` — use Bash to extract it: `python3 -c "import pandas as pd; df = pd.read_excel('FILEPATH'); print(df.to_csv(index=False))"` replacing FILEPATH with the actual path. If pandas is not available, ask the PM to export that file as CSV and re-run.

For each file, build an internal record:
- FILE_NAME — the filename
- WHAT_IT_MEASURES — what domain or topic this data covers (e.g. revenue, support tickets, conversion funnel, infrastructure costs)
- RELEVANT_TEAMS — which of the five teams (Leadership, Sales, Customer Support, Engineering, Operations) would find this data directly useful. A file can be relevant to multiple teams.
- KEY_SIGNALS — the most important trends, anomalies, or standout numbers in this file

A file can and should inform multiple team sections if the data is relevant to more than one team. Do not force a 1:1 mapping between files and teams.

If no data exists for a particular team across all files, that team's section will be omitted from the narrative — do not fabricate content.

Do not output anything yet.

## Step 2 — Gather business context

Ask the PM these two questions, one at a time. Wait for a full answer to each before asking the next.

**Question 1:** "What question does this data answer, and what's the business context — what's happening right now that makes this data relevant?"

Wait for the answer. Store it as BUSINESS_CONTEXT.

**Question 2:** "What do you want stakeholders to do or decide after reading this?"

Wait for the answer. Store it as DESIRED_ACTION.

**Question 3:** "What is the financial consequence if this data is ignored — or acted on? Even a rough estimate or directional answer works."

Wait for the answer. Store it as MONEY_CONSEQUENCE.

## Step 3 — Write the narrative

Write the narrative document using the structure below. Strict rules:
- The Recommendation leads. It is the conclusion, stated plainly before any evidence. One sentence: what should be done and why it is urgent.
- Each team section is the argument that supports the recommendation from that team's perspective. 1–3 sentences max.
- No bullets, no headers within sections, no data tables.
- Write in plain business language. No jargon, no metric names without context.
- Make it memorable — each section should have one clear takeaway that sticks.
- Ground every claim in the actual data. Do not invent implications.
- Follow the Money Stories structure throughout: what is happening → how it connects to money → why acting now matters. Use MONEY_CONSEQUENCE to anchor financial claims.
- Each team section ends with a single italic source line listing the file(s) it draws from: `*Source: filename.csv*` or `*Sources: file1.csv, file2.xlsx*`
- Omit any team section for which no relevant data exists across all files. Do not write placeholder text for missing sections.

---

```
# [Title — one line summarising what the data shows and why it matters]

*[Product or context name] · [Date range of the data if known] · Generated [YYYY-MM-DD]*

---

## Recommendation

[1–2 sentences. State the decision or action directly — no setup, no hedging. Lead with what should happen and anchor it in financial consequence using MONEY_CONSEQUENCE. This is the conclusion; the sections below are the evidence.]

---

## For Leadership

[1–3 sentences. Lead with the action this team should take — one direct imperative. Then the financial or strategic argument that supports it. Never end on the action; end on the evidence.]

*Source: [filename(s)]*

---

## For Sales

[1–3 sentences. Lead with the action. Then the revenue or pipeline argument that justifies it.]

*Source: [filename(s)]*

---

## For Customer Support

[1–3 sentences. Lead with the action. Then the volume, pattern, or experience data that supports it.]

*Source: [filename(s)]*

---

## For Engineering

[1–3 sentences. Lead with the action. Then the technical signal from the data that points to where the problem lives.]

*Source: [filename(s)]*

---

## For Operations

[1–3 sentences. Lead with the action. Then the capacity or process consequence that makes it urgent.]

*Source: [filename(s)]*
```

---

## Step 4 — Save the output

Save the completed narrative to `data-narrative/output/narrative_YYYY-MM-DD.md` using today's date.

Confirm the file path to the PM and ask: "Any sections to revise before you share this?"
