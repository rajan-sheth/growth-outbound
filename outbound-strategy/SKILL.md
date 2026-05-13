---
name: outbound-strategy
description: Build a complete outbound strategy document for any B2B company. Collects company context, ICP criteria, and market positioning upfront through guided questions, then researches target market segments, scores them on fit and opportunity, identifies lead use cases per segment, and generates a polished strategy .docx with segment-specific outbound messaging guides. Use this skill whenever a company needs to identify which market segments to target with outbound sales, build a GTM strategy, create a structured outbound playbook, prioritize verticals, or answer "who should we go after first and why." Trigger when the user says "outbound strategy," "which segments should we target," "GTM strategy," "outbound playbook," "who to target," or asks for a strategy document for a sales or marketing initiative.
---

# Outbound Strategy Skill

This skill builds a complete outbound strategy for any B2B company — from raw context to a finalized, client-ready strategy document. It works for any product, any market, any ICP.

## What it produces

A strategy `.docx` (8–12 pages) covering:
1. **ICP Profile** — the ideal target company defined precisely, including anti-ICP
2. **Market Segment Map** — which segments to pursue and why
3. **Segment Scoring** — top segments ranked by opportunity, reachability, and product fit
4. **Lead Use Cases** — the specific problems the product solves in each priority segment
5. **Outbound Messaging Guide** — persona-specific opening angles for each priority segment
6. **Recommended Next Steps** — immediate actions after the strategy is complete

---

## Step 1: Collect company context

Before researching or writing anything, collect all required inputs from the user. Use `AskUserQuestion` to ask. Do not proceed to research until you have answers to all required fields.

Group questions logically to minimize back-and-forth. You can ask in 2–3 rounds.

> **If the user has a positioning doc, ICP doc, or `product-marketing-context.md` in their workspace**, read it first and use it to pre-fill what you can. Confirm key details before proceeding. Ask only for what's missing.

### Round 1 — Company and product basics

Ask:
- **Company name and website**
- **One-line product description** — what it does and for whom (e.g., "a platform that automates X for Y type of company")
- **The core problem it solves** — the pain the buyer feels, in their language, not product language
- **What market or industry does your product serve?** — who are the buyers at a high level? (e.g., "mid-market SaaS companies," "healthcare providers," "financial services firms," "e-commerce brands")
- **2–3 strongest differentiators** vs. the most common alternatives (including doing nothing or building in-house)
- **Proof points or customer outcomes** — any measurable results, even directional (e.g., "customers cut X by 40%," "time to Y dropped from months to days"). Include named customers if they're public.

### Round 2 — ICP and targeting criteria

Ask:
- **Target company stage** — e.g., Series A+, growth-stage, mid-market, enterprise, or a mix
- **Target company size** — headcount or revenue range (e.g., 50–500 employees, $10M–$100M ARR)
- **What signals indicate a good fit?** — technical, behavioral, or business signals (e.g., "has a dedicated ops team," "recently raised funding," "uses Salesforce," "hiring for X roles," "launched a product that does Y")
- **What signals indicate a poor fit?** (anti-ICP) — be specific; this is as important as the positive criteria
- **Geography focus** — North America, EMEA, global, specific regions
- **Buyer persona titles** — the specific job titles to reach in outbound (e.g., "VP of Operations," "Head of Data," "CTO," "Director of Finance")

### Round 3 — Competitive and vertical context

Ask:
- **Main competitors** (2–3) and the client's key differentiation vs. each
- **Any segments where you already have traction?** — even 1–2 early customers signals what's working; use this to anchor the research
- **Any segments to exclude?** — regulatory, partnership conflicts, strategic decisions, etc.
- **Is there a specific outcome this strategy is meant to support?** — e.g., "prepare for a Series B raise," "build an outbound function from scratch," "enter a new market," "support a sales team of X reps"

---

## Step 2: Research target market segments

Once context is collected, research the client's market to identify the best-fit segments for outbound. Use `WebSearch` to look for:
- How analysts and industry publications segment the client's target market
- Which sub-segments are growing fastest or have the most active buyers right now
- Recent funding, hiring, and product signals that indicate urgency or buying activity
- Competitive activity by segment (where competitors are winning, where there are gaps)

### Segment identification approach

Do not use a pre-set list of verticals. Derive segments from the user's market context.

1. **Start from the market the user defined** — if they said "mid-market SaaS companies," your segments might be: SaaS in Financial Services, SaaS in Healthcare, SaaS in Retail/E-commerce, SaaS in HR/People Ops, etc.
2. **Cross with the ICP profile** — which sub-segments have the highest concentration of companies matching the user's stage, size, and fit signals?
3. **Validate with research** — use web search to find real companies in each candidate segment and test whether they match the ICP. Segments with many matching companies score higher.
4. **Aim for 5–8 candidate segments** to evaluate, then narrow to top 3–5 after scoring.

For each candidate segment, gather:
- Approximate count of companies matching the ICP in that segment
- Buying urgency signals (are they actively spending on solutions like this?)
- Competitive density (how contested is this space?)
- Reachability (are decision-makers easy to identify and reach?)

---

## Step 3: Score and rank segments

Score each candidate segment on three dimensions (1–10 scale):

| Dimension | Weight | What it measures |
|-----------|--------|-----------------|
| **Opportunity** | 40% | How large and urgent is the problem for buyers in this segment? Is there active spend, clear ROI, and a buying trigger? |
| **Reachability** | 30% | How easy is it to identify, reach, and engage target companies? (Signal density, LinkedIn presence, network density, event presence) |
| **ICP Fit** | 30% | How closely does the typical company in this segment match the client's ideal customer profile? |

Compute a weighted score for each segment. Rank them and identify top 3–5. For segments that scored well on two dimensions but not the third, note what would need to be true for them to move up.

---

## Step 4: Generate the strategy document

Build a `.docx` using the `python-docx` library. Follow this structure.

### Document structure

**Cover page**
- Client company name
- "Outbound Strategy: [brief market description, e.g., 'Mid-Market SaaS' or 'Enterprise Healthcare']"
- Date
- Prepared for / prepared by (if the user provided this context)

---

**Section 1: Executive Summary** (~1 page)
- 3–4 sentences on the market opportunity as you've researched it
- Top 3 recommended segments with one-sentence rationale each
- Expected output: approximate number of target companies and priority accounts this strategy supports
- The single most important insight from the research (the thing that would change how the client thinks about their outbound)

---

**Section 2: ICP Profile**

Table format:

| Criteria | Description |
|----------|-------------|

Rows:
- Company stage (must-have)
- Company size / headcount (must-have)
- Geography
- Technical / operational signals (must-haves)
- Behavioral / business signals (nice-to-haves)
- Anti-ICP: who to skip — be specific and actionable

---

**Section 3: Market Segment Analysis**

For each of the top 3–5 segments, write a mini-brief (~150–200 words + scoring table):

- **Segment overview**: what's happening in this space and why it's relevant for the client's product right now
- **Key buying signal**: what to look for when qualifying a company in this segment
- **Scoring breakdown** (table):

| Dimension | Score (1–10) | Rationale |
|-----------|-------------|-----------|
| Opportunity | | |
| Reachability | | |
| ICP Fit | | |
| **Weighted Total** | | |

- **2–3 example companies** in this segment (real, publicly known)
- **Why they buy now** (urgency trigger) vs. why they might wait

---

**Section 4: Lead Use Cases**

For each priority segment, 2–3 specific use cases where the client's product creates tangible value:

| Use Case | The Problem | Measurable Outcome | Proof Point or Analogy |
|----------|------------|-------------------|----------------------|

Use the client's actual proof points where possible. For segments without direct proof, use analogies from adjacent segments or market data — and label them clearly as analogies, not fabricated outcomes.

---

**Section 5: Outbound Messaging Guide**

For each priority segment, provide messaging for the top 1–2 buyer personas the user specified:

| Element | Content |
|---------|---------|
| **Target Persona** | Job title + the lens through which they see the world |
| **Core Pain** | The specific frustration this persona feels that the product addresses |
| **Opening Line** | A cold outbound opener (email or LinkedIn) — direct, specific, no filler |
| **Value Angle** | What matters to them specifically, not what the product does generically |
| **Proof Point** | The most relevant outcome or signal for this persona |
| **CTA** | A low-friction ask (15-min call, relevant resource, specific question) |

**Opening line guidelines:**
- Lead with their pain, not the product
- Reference something specific to their segment or company type
- Under 25 words for the first sentence
- No "I hope this finds you well," "I wanted to reach out," or superlatives
- Write like a practitioner talking to a peer, not a vendor pitching a buyer

---

**Section 6: Recommended Next Steps**

- Priority order for segment targeting (which to hit first and why)
- Target account research approach (what signals to look for when building a list)
- First-wave outreach sequence suggestion (channels, cadence, volume per week)
- 2–3 hypotheses to test in a pilot outreach run
- The milestone that would validate or invalidate this strategy

---

### Formatting standards

- Consistent heading hierarchy (H1 for sections, H2 for subsections)
- Tables for scoring matrices and messaging guides
- Prose paragraphs for analysis and narrative — no bullet point walls
- No buzzwords: avoid "leverage," "unlock," "seamlessly," "game-changing," "revolutionary"
- Numbers and specifics wherever possible — vague statements undermine credibility with buyers
- Total length: 8–12 pages

---

## Step 5: Save and deliver

Save the `.docx` to the user's workspace with the filename:
`[CompanyName]-outbound-strategy-[YYYY-MM].docx`

Provide a file link. Then ask:
- "Want to adjust the segment selection or scoring weights before we finalize?"
- "Ready to build the target account list? The **outbound-targeting-list** skill can take this strategy as input and build a researched, enriched prospect list from it."

---

## Quality standards

**Segment research must be grounded in real data.** A generic segment list with no company names, no funding signals, and no market evidence is not useful. Use web search to find real signals — funding rounds, job postings, product announcements, industry reports. Specificity is what makes this document credible.

**Messaging must start from buyer pain, not product features.** Every opening line and value angle should reflect something the buyer feels, not a product capability. The product earns its way in after you've established the pain.

**Anti-ICP is not optional.** A vague anti-ICP ("companies too small") wastes the sales team's time. Make it specific: what stage signals are a waste of time, what company profile almost looks right but isn't, what behavioral signals indicate a poor fit.

**Don't fabricate proof points.** If the client hasn't provided a specific outcome, write "companies in comparable situations have seen X" or reference market data rather than attributing outcomes to unnamed clients.

**Scoring rationale matters more than the score.** A score without a written rationale is guesswork. Every score in the segment table should have a one-sentence explanation of what drove it.
