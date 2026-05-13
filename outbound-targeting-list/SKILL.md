---
name: outbound-targeting-list
description: Build a researched, enriched, outbound-ready target account list for any B2B company. Collects ICP criteria, target segments, exec roles, and custom intelligence requirements upfront through guided questions, then researches real companies, scores each for ICP fit, enriches executive contacts with LinkedIn profiles, and generates a formatted .xlsx ready for outbound use. Falls back to Exa or targeted web search for any missing contact data. Use this skill whenever a company needs a researched target account list, executive contact enrichment, ICP-scored prospect database, or outbound list for any market or product. Trigger when user says "target list," "prospect list," "outbound list," "target accounts," "build a list," "who should we reach out to," "enrich contacts," or "account list."
---

# Outbound Targeting List Skill

This skill builds a researched, enriched, outbound-ready target account list — from ICP context to a formatted `.xlsx` with company profiles, ICP scoring, executive contacts, and custom intelligence columns defined by the user.

Works for any company, any product, any market.

## What it produces

A `.xlsx` file with:
- **Sheet 1 — Target Companies**: Full account list with ICP scoring and context (one row per company)
- **Sheet 2 — Priority Contacts**: Exec contact details for high-fit accounts
- **Sheet 3 — Summary**: Coverage stats and enrichment quality report

---

## Step 1: Collect company context

Use `AskUserQuestion` to collect all required inputs before starting research. Group questions to reduce back-and-forth. Do not begin research until you have answers to all required fields.

> **If the user has already run the `outbound-strategy` skill**, read that output document and pre-fill what you can from it — segments, ICP criteria, personas. Ask only for what's missing.

> **If the user has a positioning doc, ICP document, or `product-marketing-context.md` in their workspace**, read it first and use it to pre-fill answers. Confirm the key details before proceeding.

### Round 1 — Company and product basics

Ask:
- **Company name and website**
- **One-line product description** — what it does and for whom
- **Core problem it solves** — the pain the buyer feels, in their language
- **2–3 strongest differentiators** vs. alternatives (including "doing nothing" or "building in-house")
- **Key proof points** — any measurable customer outcomes, even directional (cost savings, time reduction, accuracy gains)

### Round 2 — Target company criteria

Ask:
- **Target segments or verticals** — which markets or industry segments to focus on (pull from strategy doc if available, or ask directly). Be specific: e.g., "mid-market SaaS in financial services," "healthcare providers," "enterprise logistics companies"
- **Company stage filter** — e.g., Series A+, growth-stage, mid-market, enterprise, public
- **Company size filter** — headcount range or revenue range
- **Geography** — North America, EMEA, global, or specific countries/regions
- **ICP fit signals** — what signals indicate a strong fit? (e.g., "has a dedicated data team," "recently raised funding," "running X type of operation," "uses Y type of tooling")
- **Anti-ICP** — who to exclude explicitly (e.g., "pre-product companies," "companies with fewer than 5 people in X function," "companies that already have a competitor solution in place")
- **How many target companies?** — total desired (e.g., 50, 100, 200) or per segment

### Round 3 — Contact and intelligence preferences

Ask:
- **Which exec roles to enrich contacts for?** Default is: CEO/Co-Founder, CTO/VP Engineering, and the head of the most relevant function (define based on the product). User can add or remove roles.
- **What do you want to know about each target company beyond the basics?** Define 1–3 custom intelligence fields — specific signals about each company that indicate fit, urgency, or buying readiness. Examples:
  - "Do they have a dedicated [X] team?" (Yes/No/Unknown)
  - "What tool or vendor do they currently use for [Y]?" (Open text)
  - "Have they recently announced or invested in [Z]?" (Yes/No/Unknown + source)
  - "What stage of [process] are they at?" (custom taxonomy)
  
  These become dedicated columns in the spreadsheet. The user defines the field name, the answer format (Yes/No, scale, open text), and what to look for when researching.

- **Any specific companies to include?** — companies the user wants to ensure are on the list regardless of research findings

---

## Step 2: Define the spreadsheet structure

Before researching, confirm the column structure based on user answers. The spreadsheet will have:

**Core columns (always present):**

| Column | Field | Format |
|--------|-------|--------|
| A | Company Name | Text |
| B | Segment / Vertical | Text |
| C | Sub-Type | More specific category within the segment |
| D | Website | URL |
| E | HQ | City, Country |
| F | Employees | Headcount band (e.g., "50–200") |
| G | Stage | Funding stage or company type |
| H | What They Do | 1–2 sentences on the company's product/service |
| I | Why They'd Buy | Analysis: what's the specific pain that maps to the client's product? |
| J | ICP Fit Score | 1–5 (color-coded — see scoring guide below) |
| K | ICP Fit Notes | 1–2 sentences rationale for the score |

**Exec contact columns** (one column per role the user selected):

| Column | Field | Format |
|--------|-------|--------|
| L+ | [Role 1] | Name + LinkedIn URL (newline-separated) |
| ... | [Role 2] | Name + LinkedIn URL |
| ... | [Additional roles] | Name + LinkedIn URL |

**Custom intelligence columns** (one column per user-defined intel field):

| Column | Field | Format |
|--------|-------|--------|
| After contacts | [Intel Field 1 Name] | Per user-defined format (Yes/No, open text, etc.) |
| | [Intel Field 2 Name] | |
| | [Intel Field 3 Name] | |

**Last column:**
- Notes / Research Gaps — flag anything incomplete or flagged for follow-up

Confirm this structure with the user (or show them the column layout) before building the spreadsheet.

---

## Step 3: Research target companies

For each selected segment, research real companies that match the ICP. Use `WebSearch` to find:
- Company lists by segment (use queries like "top [segment] companies 2024 2025," "[segment] startups funded," "[segment] companies [geography]")
- Industry roundups, analyst lists, and funded company databases
- LinkedIn company searches for size and stage signals
- Recent funding announcements within each segment

### Per-company data to collect

For each company, gather:

| Field | Notes |
|-------|-------|
| Company name | |
| Segment | Which of the user's target segments does this fit? |
| Sub-type | More specific category within the segment |
| Website | |
| HQ | City, Country |
| Employees | Headcount estimate (LinkedIn is the primary source) |
| Stage | Funding stage or type (Seed, Series A–D, Growth, Public, PE-backed, bootstrapped) |
| What they do | 1–2 sentences — not their marketing copy, an objective description |
| Why they'd buy | Your analysis: what specific pain does the client's product address for this company? Be concrete. |
| ICP Fit Score | 1–5 (see scoring guide) |
| ICP Fit Notes | 1–2 sentences explaining the score — what drove it up or down |

### ICP fit scoring guide

Score based entirely on the ICP criteria the user provided in Step 1.

| Score | Criteria |
|-------|----------|
| 5 — Perfect | Matches all of the user's must-have criteria; clear, acute pain that maps directly to the product; right stage, size, and geography |
| 4 — Strong | Matches most criteria; one minor gap that doesn't disqualify (slightly out of size range, adjacent use case) |
| 3 — Moderate | Matches core criteria but missing a key signal (one must-have is unconfirmed or unclear) |
| 2 — Weak | Some overlap but significant gaps against the ICP criteria |
| 1 — Stretch | Included for coverage only; would need substantial qualification before outreach |

Target distribution: ~20% fives, ~40% fours, ~30% threes, ~10% twos and ones.

---

## Step 4: Enrich executive contacts

For each company with ICP fit score ≥ 3, find executive contacts for the roles the user specified.

### Contact data to collect per exec

| Field | Format |
|-------|--------|
| Full name | First Last |
| Title | Exact current title |
| LinkedIn URL | `linkedin.com/in/handle` |
| Confidence | Confirmed (URL verified from search result) or flagged for manual search |

### Enrichment approach — three layers

**Layer 1 — Web search:**
Search `"[Full Name] [Company Name] LinkedIn"` and look for direct LinkedIn profile URL matches in search result snippets, company press releases, team pages, or professional profile pages. Only use a URL you can confirm from a source — do not infer or construct handles.

**Layer 2 — Exa connector (if available):**
Check whether an Exa search connector is connected (look for tools with `exa` in the name or tools matching `mcp__*__search`). If available, use it for:
- `"[Full Name]" "[Company]" site:linkedin.com`
- `"[Company]" "[Title or function]" site:linkedin.com`
- `"[Company]" "head of [function]" linkedin`

**Layer 3 — Fallback marker (do not fabricate):**
If a LinkedIn URL cannot be confirmed from any source, write:
`[Full Name]\n[Search: "[Full Name] [Company] LinkedIn"]`

This preserves the ability to manually verify rather than chasing a wrong URL. A wrong URL destroys trust with the sales team faster than an empty cell.

### Role lookup approach

For each exec role the user specified:
1. Check the company's "About" or "Team" page first — fastest source for confirmed names and titles
2. Search LinkedIn for `[Company] [Title]`
3. Check recent press releases, funding announcements, or media coverage — executives are often named

If a company uses a combined role (e.g., "Co-Founder & CTO"), include it once under the most relevant category for the client's outreach.

---

## Step 5: Research custom intelligence columns

For each user-defined intelligence field, research each company (ICP fit ≥ 3) and fill in the answer.

### Research approach per field type

**Yes/No/Unknown fields** (e.g., "Do they have a dedicated data team?"):
- Search `"[Company]" "[field keyword]"` and look for job postings, team pages, blog posts, or press releases that confirm or deny
- If no evidence found, mark as "Unknown" with a note on what was searched
- Do not mark "No" unless you've found confirming evidence that they don't have this; "Unknown" is more accurate

**Open-text fields** (e.g., "What tool do they currently use for X?"):
- Search for job postings (these often name the stack), press releases, case studies, or G2/Capterra reviews
- Provide the answer + the source if found
- Mark "Not found" if no evidence available

**Taxonomy fields** (user-defined categories):
- Research to determine which category applies and provide a brief rationale + source

For all intelligence fields: **source your findings**. Either provide a URL in the Notes column or a brief description of where the data came from. Unsourced intelligence is hard to trust in a sales context.

---

## Step 6: Build the .xlsx file

Use `openpyxl` in Python to build the spreadsheet. Write and run the Python code via bash. Structure the script based on the column layout confirmed in Step 2.

### Formatting standards

**Title row (row 1):**
- Merged across all columns
- Text: `[Client Company Name] — Outbound Target List | [Month Year]`
- Background: `#2E4057`, font: white, bold, 14pt

**Header row (row 2):**
- Background: `#1F3864` (dark navy)
- Font: white, bold, 10pt
- Freeze this row

**ICP Fit Score color coding:**
- Score 5: fill `#C6EFCE`, font `#276221` (dark green)
- Score 4: fill `#E2EFDA` (light green)
- Score 3: fill `#FFEB9C`, font `#9C6500` (yellow/amber)
- Score 2: fill `#FCDAC3` (light orange)
- Score 1: fill `#FFC7CE`, font `#9C0006` (red)

**Custom intelligence columns — Yes/No/Unknown color coding:**
- "Yes" / confirmed: fill `#C6EFCE`
- "Planned" / in progress: fill `#FFEB9C`
- "No" / not present: fill `#FFC7CE`
- "Unknown": no fill

**General formatting:**
- Wrap text enabled for all descriptive columns (What They Do, Why They'd Buy, ICP Fit Notes, intel columns)
- All contact columns: wrap text, newline between name and LinkedIn URL
- Column widths: narrow for codes (Score, Stage), medium for names/websites, wide for descriptive text
- Alternating row shading (very light grey on even rows) for readability
- Bold company names in column A

### Sheet 2 — Priority Contacts

Include only companies with ICP fit score 4–5. Columns:
- Company | Segment | ICP Fit Score | [One column per exec role — Name] | [One column per exec role — LinkedIn URL] | Notes / Research Gaps

### Sheet 3 — Summary

Table format including:
- Total companies by segment
- ICP fit score distribution (count and % per tier)
- Custom intelligence field coverage (% with confirmed answer vs. Unknown)
- Contact enrichment rate (% of ICP 4–5 companies with ≥ 1 confirmed LinkedIn)
- Date generated

---

## Step 7: Enrichment gap review

After building the initial file, review exec contact columns for all ICP 4–5 companies.

For any company with zero confirmed contacts:
1. Try one additional web search pass with different query formats
2. If Exa is available, run a targeted search
3. Log these companies in the Summary sheet under "Flagged for Manual Research"

Do not mark enrichment complete if high-fit accounts are blank — these are the accounts that matter most.

---

## Step 8: Save and deliver

Save the `.xlsx` to the user's workspace with the filename:
`[CompanyName]-outbound-targets-[YYYY-MM].xlsx`

Provide a file link.

After delivery, report:
- Total companies in the list
- Breakdown by segment
- Number with ICP fit score 4–5
- Contact enrichment rate (% ICP 4–5 accounts with ≥ 1 confirmed LinkedIn)
- Any segments where research was thin (flag for follow-up)
- Any custom intelligence fields with low coverage

Then ask:
- "Want me to add more companies to any specific segment?"
- "Should I prioritize a first-wave outreach sequence from the top accounts?"
- "Any contacts that came back empty — want me to try a different search approach?"

---

## Quality standards

**Depth over volume.** A list of 60 well-researched, scored accounts with real context beats a list of 200 generic company names. If research takes longer because you're being thorough on the right accounts, that's the right tradeoff.

**ICP scoring requires reasoning.** Every score needs a rationale in the ICP Fit Notes column. A score without reasoning is guesswork and isn't useful for qualification calls.

**No fabricated contact data.** Wrong LinkedIn URLs and incorrect names actively harm the sales team. If a URL isn't confirmed, use the `[Search: ...]` marker. This is a feature, not a gap — it hands off a clear action to the researcher.

**Intelligence columns need sourcing.** Unsourced intel ("they use Salesforce" with no evidence) is a liability, not an asset. Every confirmed intelligence finding should reference where it came from — even a brief note like "from job posting" or "from company blog, 2024."

**Prioritize enrichment on best-fit accounts.** Spend research time on ICP 4–5 companies. For threes, basic firmographics are enough. Don't spend enrichment cycles on twos and ones.

**"Why they'd buy" is analysis, not description.** The column should say why this specific company has a pain that maps to the client's product — not just what the company does. This is the column that makes the list useful for personalized outreach.
