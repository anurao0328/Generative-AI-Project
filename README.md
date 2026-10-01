# Generative AI Customer Feedback Analysis Capstone

## Project Overview

This capstone explores how generative AI and structured prompt engineering can turn customer review text into organized, evidence-grounded insights. The primary analysis scope is feedback from Amazon and Yelp. Excel workbooks support data preparation and organization of analysis outputs, and the project includes a written prompt-engineering guide and presentation materials.

The workflow is designed to identify recurring feedback themes, explain sentiment, investigate possible causes, and develop business recommendations. AI-generated interpretations should be checked against the source reviews, and inferred causes should be distinguished from directly supported evidence.

## Project Objectives

- Analyze positive and negative customer feedback from Amazon and Yelp.
- Use reusable prompts to structure the analysis consistently.
- Ground themes and explanations in review evidence.
- Separate observations from assumptions during root-cause analysis.
- Translate feedback patterns into prioritized recommendations and an executive-level summary.

## Datasets

The repository includes the following labelled review-sentence datasets:

- **Amazon:** `amazon_cells_labelled.txt` and `Amazon_Data.xlsx`
- **Yelp:** `yelp_labelled.txt` and `Yelp_Data.xlsx`
- **Combined/working data:** `Complete_Data.xlsx`

The accompanying dataset notes describe each source as containing 1,000 sentences: 500 positive and 500 negative, with labels `1` for positive and `0` for negative. The raw text format is one sentence and one sentiment score per tab-separated row. The supplied sentiment-labelled-sentences notes identify Amazon, Yelp, and IMDb as the three source domains. An IMDb raw file is present in the repository, but IMDb is not listed as part of the primary Amazon-and-Yelp analysis scope here.

## Problem Statement

Customer reviews contain useful signals about satisfaction and pain points, but raw text is difficult to compare and act on consistently. This project uses a structured generative-AI workflow to organize Amazon and Yelp feedback into themes, explain sentiment, explore plausible operational causes, and produce recommendations. Review evidence is the basis for findings; proposed causes that are not explicit in the text are treated as assumptions rather than facts.

## Prompt Engineering Methodology

The project guide describes a reusable, multi-stage prompt workflow. It recommends processing review data in manageable batches, requiring consistent tabular outputs, grounding claims in review text, and spot-checking AI-generated patterns against the source data.

The requested analysis stages are:

1. **Theme Extraction:** Identify recurring positive and negative themes, supporting examples, business meaning, and approximate frequency.
2. **Sentiment Explanation:** Explain the language behind sentiment labels and flag ambiguous, mixed, or potentially sarcastic examples.
3. **Root-Cause Analysis:** Suggest plausible causes for negative themes while separating review evidence from inference and assigning confidence.
4. **Recommendation Generation:** Propose and prioritize actions linked to themes and supporting evidence.
5. **Executive Summary:** Present the scope, major insights, actions, and limitations concisely for a business audience.

The project guide also describes a prompt chain in which theme outputs inform root-cause analysis, recommendations, and the executive summary. The workflow is prompt-based; the guide does not describe model training or a custom software application.

## Analysis Workflow

1. Review the problem statement and summarize the business question.
2. Prepare the Amazon and Yelp feedback data in Excel, retaining the review text, sentiment label, and source.
3. Process the feedback in batches using structured prompts.
4. Extract themes and explain sentiment, then analyze likely root causes.
5. Generate recommendations tied to the themes and evidence.
6. Spot-check examples against original reviews and note limitations.
7. Compile findings into a concise executive summary and presentation.

## Project Deliverables

Files supplied as project deliverables and working materials include:

- Prompt and workflow reference: `Final_Project_Files/GenAI_Capstone_Prompt_Engineering_Guide.md`
- Analysis workbooks: `Data_Files/Prompt_2_Results.xlsx`, `Data_Files/Prompt_2_Prompt_4.xlsx`, and `Data_Files/Prompt_2_Prompt_4_Prompt_5.xlsx`
- Data workbooks: `Data_Files/Amazon_Data.xlsx`, `Data_Files/Yelp_Data.xlsx`, and `Data_Files/Complete_Data.xlsx`
- Capstone workbook: `Final_Project_Files/Capstone Gen AI.xlsx`
- Written insights report: `Final_Project_Files/Customer_Feedback_Insights.pdf`
- Presentation: `Final_Project_Files/Customer_Feedback_Insights.pptx`
- Original problem statement: `Problem_Statement_With_Raw_Data/GenAI_Capstone_Project_Problem_Statement.pdf`
- Raw labelled datasets and accompanying notes in `Problem_Statement_With_Raw_Data/`

## Project Structure

```text
Generative_AI_Capstone_Project/
|-- README.md
|-- Data_Files/
|   |-- Amazon_Data.xlsx
|   |-- Complete_Data.xlsx
|   |-- Prompt_2_Prompt_4.xlsx
|   |-- Prompt_2_Prompt_4_Prompt_5.xlsx
|   |-- Prompt_2_Results.xlsx
|   `-- Yelp_Data.xlsx
|-- Final_Project_Files/
|   |-- Capstone Gen AI.xlsx
|   |-- Customer_Feedback_Insights.pdf
|   |-- Customer_Feedback_Insights.pptx
|   `-- GenAI_Capstone_Prompt_Engineering_Guide.md
`-- Problem_Statement_With_Raw_Data/
    |-- GenAI_Capstone_Project_Problem_Statement.pdf
    |-- amazon_cells_labelled.txt
    |-- imdb_labelled.txt
    |-- readme.txt
    `-- yelp_labelled.txt
```

## Tools and Technologies

- **Generative AI / large language models:** Prompt-based review analysis, as described in the project guide.
- **Microsoft Excel:** Data preparation, batching, and organization of analysis outputs.
- **Microsoft PowerPoint:** Presentation deliverable.
- **Markdown:** Project prompt-engineering guide and this README.

The repository does not identify a specific LLM provider as the one used to produce the project outputs. The guide mentions ChatGPT and Claude as examples.

## Key Outcomes

- A structured prompt workflow for analyzing customer feedback across multiple stages.
- Excel workbooks covering source data and prompt-analysis stages.
- A customer-feedback insights report and presentation.
- A documented approach that emphasizes evidence checks, explicit assumptions, and limitations.
Specific quantitative findings are not reproduced here; consult the supplied report, presentation, and workbooks for the project analysis.

## Conclusion

This capstone demonstrates a prompt-engineering approach to organizing customer feedback into themes, explanations, possible causes, and actionable recommendations. Its emphasis on consistent outputs and review-level validation supports more transparent use of generative AI in customer-experience analysis.

## Author

ANUSHA
