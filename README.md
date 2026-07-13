# Productiser

A portfolio of AI-native product management tools built with Claude Code. Each skill is a slash command that handles a specific, repeatable PM job — research, synthesis, communication.

---

## Skills

### [Competitive Analysis](competitive-analysis/)

Runs a structured competitive analysis for any product. Discovers competitors, researches features, pricing, positioning, and distribution, then generates a full report with month-over-month change tracking.

`/analyse-competitors`

---

### [Data Narrative](data-narrative/)

Turns raw analytics data into a stakeholder narrative structured around a clear recommendation. Based on Rich Mironov's Money Stories framework — anchors every narrative in financial consequence, not data observation. One document, five team audiences.

`/tell-the-story`

---

### [Discovery Synthesis](discovery-synthesis/)

Turns raw discovery inputs — interview notes, call transcripts, support tickets — into structured insight cards. Identifies distinct problems across all sources, groups related signals, and rates confidence based on consistent criteria. One card per problem, ordered by confidence.

`/map-insights`

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

### 3. Add your input files

Each skill has its own `input/` folder — see the skill's README for the expected format. All `input/` and `output/` folders are gitignored and never committed.

### 4. Run

```bash
claude
```

Then type the skill name. See each skill's README for full usage.

---

## Structure

```
productiser/
├── .claude/
│   └── commands/              # skill definitions (Claude Code slash commands)
├── competitive-analysis/      # competitive analysis skill
│   ├── README.md
│   ├── input/                 # gitignored — your private inputs
│   └── output/                # gitignored — generated reports
├── data-narrative/            # data to stakeholder narrative skill
│   ├── README.md
│   ├── input/
│   │   └── data/              # drop data files here
│   └── output/
├── discovery-synthesis/       # discovery synthesis skill
│   ├── README.md
│   ├── input/                 # drop notes, transcripts, tickets here
│   └── output/
└── README.md
```

Each skill is self-contained: its own inputs, outputs, and documentation.
