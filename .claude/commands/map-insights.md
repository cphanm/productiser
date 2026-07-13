You are synthesising discovery inputs into structured insight cards. Follow these steps in order.

## Step 1 — Load all discovery inputs

Read all files in `discovery-synthesis/input/`. If the directory is empty or does not exist, ask the PM to drop their notes or transcripts there and re-run.

Treat all files equally — interview notes, call transcripts, and support tickets are the same raw material. For each file, note:
- FILE_NAME — the filename
- INPUT_TYPE — inferred type: interview notes, transcript, support ticket, or other
- RAW_SIGNALS — a list of pain points, frustrations, unmet needs, workarounds, or surprises observed in the file. Quote directly where possible.

Do not output anything yet.

## Step 2 — Gather context

Ask the PM one question:

**"What product area or research question does this discovery cover? This helps frame the problem statements accurately."**

Wait for the answer. Store it as DISCOVERY_CONTEXT.

## Step 3 — Identify distinct problems

Across all files and their RAW_SIGNALS, identify distinct problems. Group related signals from different sources into the same problem — different people describing the same friction is one problem, not many.

Use DISCOVERY_CONTEXT to frame problems accurately.

For each distinct problem, prepare internally:
- A one-sentence problem statement
- Which files it appears in
- How many independent sources raised it
- Whether it was observed behaviour or stated preference

Do not output insight cards yet.

## Step 4 — Approval gate: problem list

Present the list of distinct problems to the PM as a numbered list. For each, show:
- The draft problem statement
- The number of sources it appeared in
- The source filenames

Then ask:
- "Any problems to merge, split, rename, or remove?"
- "Any problems missing that you expected to find?"

Wait for the response. Update the list based on feedback.

## Step 5 — Generate insight cards

For each approved problem, write an insight card using this structure:

```
## [Problem statement — one sharp, specific sentence. No hedging, no jargon.]

**Evidence:** [What was said or observed. Quote directly from the source where possible. Do not paraphrase into vagueness — keep the specific language the user used.]
*Sources: filename, filename*

**Confidence:** [High / Medium / Low]

**Why:** [One sentence explaining the confidence rating, based on the criteria below.]

**Recommended next step:** [One specific, actionable step. Not "do more research" unless that is genuinely the right move. Name the action, the owner if obvious, and the expected outcome.]
```

Confidence criteria — apply consistently:
- **High** — raised by multiple independent sources, or described with specific frequency or business impact, or observed as actual behaviour (not just stated preference)
- **Medium** — raised by one source with meaningful context, or by multiple sources but described vaguely
- **Low** — mentioned once, anecdotally, without specificity, frequency, or impact

Rules:
- Problem statement leads with the user's reality, not the product's feature gap. Write what the person experiences, not what is missing from the product.
- Evidence must be grounded in the actual inputs. Do not invent or generalise beyond what the sources say.
- Recommended next step must be concrete. Avoid generic advice.
- Order cards from highest to lowest confidence.

## Step 6 — Save the output

Write all insight cards to `discovery-synthesis/output/synthesis_YYYY-MM-DD.md` using today's date.

Prepend a short header before the cards:

```
# Discovery Synthesis — [DISCOVERY_CONTEXT]

*Generated [YYYY-MM-DD] · [N] problems identified · Sources: [list of filenames]*

---
```

Confirm the file path to the PM and ask: "Any cards to revise before you share this?"
