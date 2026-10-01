# GenAI-Powered Customer Feedback Insights Assistant
## A Prompt-Engineering-Only Capstone Guide (Amazon + Yelp Datasets)

This guide shows you how to build the entire capstone **using prompts as the engine** — no coding or ML model training required. Excel is used only as a lightweight "data prep + output organizer," and an LLM (Claude, ChatGPT, etc.) does all the analytical thinking through a structured prompt library.

---

## 1. The Design Philosophy (How a Prompt Engineer Approaches This)

Instead of writing code to cluster themes or run sentiment models, you treat the **LLM as your analyst**. The trick to making this rigorous (not just "chatting with AI") is:

1. **Chunk the data** — LLMs work best on batches of ~50–150 review rows at a time, not all 2,000 at once. You run the same prompt repeatedly across chunks, which is exactly what makes the workflow "reusable."
2. **Fix the output format** — every prompt forces a structured table/JSON output so results from different batches and different reviewers can be merged consistently in Excel.
3. **Separate evidence from inference** — every prompt explicitly asks the model to quote/paraphrase real review lines as evidence before making a business claim. This satisfies the rubric's "ground explanation in review text" requirement and reduces hallucination risk.
4. **Chain prompts like a pipeline** — the output of Prompt 1 (theme extraction) becomes the input of Prompt 4 (root-cause) becomes the input of Prompt 5 (recommendations) becomes the input of Prompt 6 (executive summary). This chaining *is* your "GenAI workflow/prototype" — no app needed.
5. **Validate with spot-checks** — after each prompt run, you re-read 5–10 source rows per theme to confirm the AI didn't invent a pattern. This becomes your "Validate" phase and your "Reflection" deliverable.

---

## 2. End-to-End Project Workflow (Mapped to the 8 Project Phases)

| Phase | What you actually do | Tool | Output |
|---|---|---|---|
| 1. Understand | Re-read the problem statement; write 3–4 lines framing the business problem in your own words | You (no AI needed) | Project understanding notes |
| 2. Dataset Summary | Open both .txt files in Excel, check row counts (1,000 each), confirm label balance (500/500) | Excel | Dataset summary |
| 3. Prepare | Combine Amazon + Yelp into one working file with `feedback_text`, `sentiment_label`, `source` columns | Excel | Working dataset (`capstone_customer_feedback.xlsx`) |
| 4. Explore & Prompt | Run **Prompt 1 (Summarization)** and **Prompt 2 (Theme Extraction)** on chunks of the data | LLM | Initial theme list |
| 5. Deepen | Run **Prompt 3 (Sentiment Explanation)** and **Prompt 4 (Root-Cause Analysis)** on the themes found | LLM | Root-cause findings |
| 6. Recommend | Run **Prompt 5 (Recommendation Generation)** using the themes + root causes as input | LLM | Recommendations table |
| 7. Validate & Summarize | Spot-check 5–10 rows per theme in Excel; run **Prompt 6 (Executive Summary)** | You + LLM | Validated findings + exec summary |
| 8. Present | Drop findings into the 6–8 slide deck template (Section 6 below) | PowerPoint | Final presentation |

---

## 3. Data Preparation in Excel (Layman's Steps)

You don't need Python for this — Excel does it in a few minutes.

**Step 1 — Open both files in Excel**
- Right-click `amazon_cells_labelled.txt` → **Open with → Excel**.
- Excel will likely dump everything into Column A. If so: select Column A → go to the **Data** tab → click **Text to Columns** → choose **Delimited** → tick **Tab** → click **Finish**. This splits each row into the review sentence (Column A) and the 0/1 label (Column B).

**Step 2 — Label the columns**
- In row 1, type headers: `feedback_text` in A1, `sentiment_label` in B1.
- Add a third column `source`. Fill every row in this sheet with the word **Amazon** (select the cell, type "Amazon", then drag the fill handle down the whole column, or type it in the first cell and double-click the fill handle to auto-fill to the last row).

**Step 3 — Repeat for the Yelp file**
- Same steps, but fill the `source` column with **Yelp** instead.

**Step 4 — Combine into one sheet**
- Copy all rows (excluding header) from the Yelp sheet and paste them directly underneath the last row of the Amazon sheet in one workbook. You now have a single 2,000-row table with three columns.
- Save this as `capstone_customer_feedback.xlsx`. This is your "working dataset" deliverable.

**Step 5 — Quick sanity checks (Dataset Summary deliverable)**
- Use `=COUNTA(A2:A2001)` to confirm 2,000 rows.
- Use `=COUNTIF(B:B,1)` and `=COUNTIF(B:B,0)` to confirm the 50/50 positive/negative split.
- Use `=COUNTIF(C:C,"Amazon")` and `=COUNTIF(C:C,"Yelp")` to confirm 1,000 rows per source.

**Step 6 — Create batches for prompting**
- Since LLMs work best on smaller batches, use Excel filters: filter `source = Amazon` and `sentiment_label = 0`, copy ~100 rows at a time into a prompt. Repeat for `= 1`, then switch source to Yelp. This gives you four natural batches per file (positive/negative × Amazon/Yelp) — ideal for theme comparisons later.

**Step 7 — Store AI outputs back in Excel**
- After running each prompt below, paste the LLM's table output into a **new sheet** (e.g., `Themes`, `RootCauses`, `Recommendations`). Because every prompt forces a fixed column structure, these sheets will merge cleanly and can be turned into pivot tables or simple bar charts (e.g., count of complaints per theme) for your slides.
- To make a quick chart: select the theme + count columns → **Insert tab → Recommended Charts → Bar Chart**.

---

## 4. The Reusable Prompt Library (6 Prompts + 1 Master Chaining Prompt)

Each prompt below is written to be **pasted as-is** into your LLM of choice, with a placeholder `[PASTE REVIEW BATCH HERE]` where you insert a chunk of rows copied from Excel (format: `sentence <TAB> label`, or just the sentences).

### Prompt 1 — Feedback Summarization
**Purpose:** Summarize Customer Reviews

```
You are a customer experience analyst reviewing a set of customer feedback sentences. Each row contains a customer review and may also include a sentiment label, where 1 means positive and 0 means negative.

Task:
1. Read and understand all the review sentences provided. 
2. Write a 4–6 sentence summary of what customers liked, based only on the positive reviews. 
3. Write a 4–6 sentence summary of what customers disliked, based only on the negative reviews. 
4. Do not add or assume any information that is not mentioned in the reviews. 
5. Finish with a one-line “Overall Sentiment Balance” showing the percentage of positive and negative feedback and the main reasons behind the sentiment. 
Source: [Amazon / Yelp]

Batch:
[PASTE REVIEW BATCH HERE]
```

### Prompt 2 — Theme Extraction
**Purpose:** Identify Common Themes.

```
You are a customer experience analyst. Review and analyze the customer feedback provided below. Identify the main recurring themes, separating positive themes (things customers liked) from negative themes (customer problems or pain points).
For each theme, provide:

- Theme Name – A short and business-friendly name. 
- Sentiment – Positive or Negative. 
- Evidence – Give 2–3 examples based on the reviews. Paraphrase the reviews without adding new information. 
- Business Meaning – Explain in one sentence why the theme is important to the business. 
- Frequency Estimate – Give an approximate count of how many reviews relate to the theme. 

Present the results in a table using these columns:
Theme | Sentiment | Evidence | Business Meaning | Frequency Estimate
Source: 
[Amazon / Yelp]

Batch:
[PASTE REVIEW BATCH HERE]
```

### Prompt 3 — Sentiment Explanation
**Purpose:** Shift the analysis from “what customers are complaining about” to “the operational reasons these issues are likely occurring” — while clearly distinguishing evidence from assumptions.

```
You are a linguistic/CX analyst. For each of the following review sentences, the original dataset label is provided (1 = positive, 0 = negative).

For each sentence:
1. State whether the AI-inferred sentiment agrees with the dataset label.
2. Explain in one sentence WHY the sentence reads as positive or negative — point to the specific words or phrases driving that read.
3. Flag any sentence that seems ambiguous, sarcastic, or mixed (these are useful for the "limitations" section of the final report).

Format as a table: Sentence | Dataset Label | AI-Inferred Sentiment | Agreement (Y/N) |
Explanation | Ambiguity Flag (Y/N)

Batch:
[PASTE REVIEW BATCH HERE]
```

### Prompt 4 — Root-Cause Analysis
**Purpose:** Move from "what customers complain about" to "why it's likely happening operationally" — while being explicit about what's evidence vs. assumption.

```
You are a Business Analyst supporting the Head of Customer Experience. Below is a set of negative themes identified from customer reviews, along with example evidence.

For each negative theme, complete the following:

1. Identify 1–2 likely root causes (operational, product, staffing, process, etc.).
2. Clearly distinguish between:
- Evidence — insights directly supported by customer review text
- Assumption — reasonable inferences not explicitly stated by customers
3. Assign a confidence rating for each root cause: High / Medium / Low.

Present your output in a table with the following columns:
Theme | Likely Root Cause | Evidence (from reviews) | Assumption (unstated inference) | Confidence

Negative themes and evidence are provided below:
[PASTE THEME TABLE OUTPUT FROM PROMPT 2 HERE]
```

### Prompt 5 — Recommendation Generation
**Purpose:** Transform insights into a prioritized set of actionable business recommendations.

```
You are an AI‑assisted Business Analyst preparing a recommendation plan for the Head of Customer Experience. Using the negative themes, root‑cause analysis, and positive themes provided, develop a prioritized action plan.

For each negative theme, propose one specific, actionable business intervention. For each positive theme, propose one action to maintain or strengthen it.

Each recommendation must include:
- Priority (High / Medium / Low), based on issue frequency and severity
- Expected Impact (what will improve if the action is implemented)
- Rationale (explicitly linked to the theme and supporting evidence)
Present the output in a table with the following columns: Theme | Recommended Action | Priority | Expected Impact | Rationale

Themes + Root Causes:
[PASTE OUTPUTS FROM PROMPT 2 AND PROMPT 4 HERE]
```

### Prompt 6 — Executive Summary
**Purpose:** Create a clear, concise narrative that a non‑technical leader can absorb and act on within two minutes.

```
You are preparing a one‑page executive summary for the Head of Customer Experience. 

Using the findings and recommendations provided, produce:
1. A two‑sentence overview defining the problem and scope (including data sources and volume reviewed).
2. The top three customer pain points — each in one line, supported by a relevant metric (e.g., “% of negative feedback”).
3. The top two strengths or positive patterns to preserve.
4. Three prioritized recommended actions — each expressed in one line.
5. A single sentence outlining the limitations of the analysis.

Keep the entire summary under 200 words, written in simple, direct business language without technical jargon.

Findings + Recommendations:
[PASTE OUTPUTS FROM PROMPTS 2, 4, AND 5 HERE]
```

### Master Chaining Prompt (Optional 7th — ties the pipeline together)
**Purpose:** Create a single, reusable GenAI workflow prompt that serves as your end‑to‑end “customer feedback insights engine” — ideal for demonstrating how the process can be repeated for future batches of feedback.

```
You are the Customer Feedback Insights Assistant. You will receive a fresh set of customer review sentences (with or without sentiment labels) from any platform (Amazon, Yelp, or any new source).

Execute the full analysis pipeline automatically and return all five outputs in sequence:
1. Summary: 4–6 sentences each for positive and negative feedback.
2. Theme Table: Theme | Sentiment Direction | Evidence | Business Meaning | Frequency
3. Root‑Cause Table: Theme | Root Cause | Evidence | Assumption | Confidence
4. Recommendations Table: Theme | Action | Priority | Expected Impact | Rationale
5. Executive Summary: A concise narrative under 200 words.

New batch:
[PASTE NEW REVIEW BATCH HERE]
```

---

## 5. Worked Example (Using Real Rows From Your Dataset)

To prove the pipeline works, here's a miniature run using a handful of actual rows from your files:

**Sample Amazon input (negative-labeled):**
- "So there is no way for me to plug it in here in the US unless I go by a converter."
- "Tied to charger for conversations lasting more than 45 minutes. MAJOR PROBLEMS!!"
- "I have to jiggle the plug to get it to line up right to get decent volume."

**Sample Yelp input (negative-labeled):**
- "Crust is not good."
- "The potatoes were like rubber and you could tell they had been made up ahead of time being kept under a warmer."
- "I was disgusted because I was pretty sure that was human hair."

Running **Prompt 2 (Theme Extraction)** on these would plausibly return themes like *"Product connectivity/compatibility issues"* and *"Charging/battery reliability"* for Amazon, and *"Food quality/freshness"* and *"Food safety/hygiene concerns"* for Yelp — each backed by the exact evidence lines above. This is the pattern you'll repeat across the full 2,000-row dataset in batches.

---

## 6. Final Presentation Structure (6–8 Slides)

| Slide | Content to pull from |
|---|---|
| 1. Title + business problem | Section 1 of the problem statement, in your own words |
| 2. Dataset overview & scope | Your Excel dataset summary (row counts, sources, label balance) |
| 3. GenAI workflow/prototype | The Master Chaining Prompt (Section 4) — show it as your "assistant design" |
| 4. Prompt strategy & examples | 2–3 of your 6 prompts, with one real input/output example |
| 5. Key findings & evidence | Merged theme tables from Amazon + Yelp (Prompt 2 outputs) |
| 6. Recommended business actions | Prompt 5 output, prioritized table |
| 7. Risks, limitations, validation | Ambiguous/sarcastic cases flagged in Prompt 3; spot-check notes |
| 8. Next steps & improvements | E.g., extend to IMDb, automate batching, add a lightweight Excel macro or app front-end |

---

## 7. Reflection Notes (Template for Deliverable #6)

Use this structure when writing your reflection:
- **Where GenAI helped:** e.g., rapid theme extraction across hundreds of sentences, consistent table formatting, drafting the executive summary tone.
- **Where it produced weak/risky outputs:** e.g., overconfident root-cause claims not directly supported by text, merging distinct themes together, missing sarcasm/negation ("not good" vs "good").
- **How you validated/improved it:** e.g., re-reading 10 sample sentences per theme, tightening prompts to require "Evidence vs Assumption" separation, running the same batch twice to check consistency.

---

## 8. Citation (Required)

Kotzias, D. (2015). *Sentiment Labelled Sentences* [Dataset]. UCI Machine Learning Repository. DOI: 10.24432/C57604.
