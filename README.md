# Growth Skills

AI-powered workflows for outbound strategy and target account list building. Built for [Claude Cowork](https://claude.ai). Works for any B2B company, any product, any ICP.

---

## Skills

| Skill | What it does | Output |
|-------|-------------|--------|
| [`outbound-strategy`](outbound-strategy/SKILL.md) | Collects your ICP and product context, researches target market segments, scores them on opportunity and fit, and generates a strategy document with segment-specific outbound messaging | `.docx` (8–12 pages) |
| [`outbound-targeting-list`](outbound-targeting-list/SKILL.md) | Researches real target companies, scores each for ICP fit, enriches exec contacts with LinkedIn, and builds custom intelligence columns you define | `.xlsx` account list |

---

## How to install

1. Download the `.skill` file for the skill you want:
   - [`outbound-strategy.skill`](outbound-strategy.skill)
   - [`outbound-targeting-list.skill`](outbound-targeting-list.skill)
2. Open Claude Cowork
3. Go to **Settings → Plugins & Skills → Install from file**
4. Select the downloaded `.skill` file
5. The skill appears in your available skills list immediately

---

## How to use

### outbound-strategy

**Say something like:**
> "Build an outbound strategy for [company name]"
> "Which market segments should we target?"
> "Create an outbound playbook for [product]"

**The skill asks you for:**
- Company name, website, one-line product description
- The core problem your product solves (in the buyer's language)
- Your strongest differentiators and any proof points / customer outcomes
- Target company stage, size, and geography
- ICP fit signals (what indicates a good fit) and anti-ICP (who to skip)
- Buyer persona titles you want to reach
- Competitor context and any segments you already have traction in

**What you get:**
A strategy document covering your ICP profile, ranked market segments with scoring, lead use cases per segment, and a persona-specific messaging guide with opening lines and CTAs for each priority segment.

---

### outbound-targeting-list

**Say something like:**
> "Build a target account list for [company name]"
> "Create a prospect list for [segment]"
> "Who should we reach out to in [market]?"

**The skill asks you for:**
- Company and product context (same as strategy skill — or paste from that output)
- Target segments, company stage/size/geography filters, anti-ICP
- Which exec roles to enrich (CEO, CTO, VP Eng, Head of [function] — you choose)
- **Custom intelligence columns** — 1–3 things you want to know about each company beyond the basics. You define the field name and what to research. Examples:
  - "Do they have a dedicated [X] team?" (Yes / No / Unknown)
  - "What tool are they currently using for [Y]?" (open text)
  - "Have they recently announced investment in [Z]?" (Yes / No / Unknown + source)

**What you get:**
A spreadsheet with company details, ICP fit scores (1–5, color-coded) with written rationale, exec contacts and LinkedIn URLs (confirmed only — unconfirmed flagged for manual research), your custom intelligence columns sourced with evidence, and a summary sheet with coverage stats.

---

## Recommended workflow

Run them in sequence for the best results:

```
1. outbound-strategy  →  Identify which segments to target and how to message them
2. outbound-targeting-list  →  Build the actual prospect list for those segments
```

Each skill also works standalone if you already know which segments to target.

---

## Files

```
outbound-strategy/
└── SKILL.md                  ← source (edit this to customize the skill)
outbound-strategy.skill       ← installable package (share this)

outbound-targeting-list/
└── SKILL.md                  ← source
outbound-targeting-list.skill ← installable package
```

To update a skill: edit the `SKILL.md`, then re-package it in Cowork using the skill-creator tool to generate a new `.skill` file.
