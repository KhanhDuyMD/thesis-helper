---
name: thesis-helper
description: Build, review, and operationalize Vietnamese medical/health-science thesis structures. Use when the user asks to write or revise a thesis proposal or thesis, create thesis titles/objectives, build opening/chapter 1/chapter 2/chapter 3/chapter 4/conclusion/reference structure, adapt the framework to different study designs, or create prompts for an R/data-analysis agent to analyze and present chapter 3 from a proposal and dataset. Handles descriptive, cross-sectional analytic, case-control, cohort, before-after, intervention, diagnostic accuracy, prognostic/prediction, survival, genetic/SNP, scale validation, qualitative, and mixed-methods theses.
---

# Thesis-Helper

## Core Workflow

1. Identify study design before writing anything.
2. Identify main analysis direction: descriptive, associated factors, treatment, diagnosis, prognosis, prediction model, survival, genetics, scale validation, qualitative, or mixed-methods.
3. Align title, objectives, chapter 2, chapter 3, chapter 4, conclusion, and recommendations.
4. Build chapter 2 before chapter 3; chapter 2 defines variables, groups, timepoints, denominators, tests, and models.
5. Use section 3.1 for general participant characteristics as a local-corpus default when appropriate; organize later sections by approved objectives, study design, and analyses performed. Do not copy a fixed sequence from examples. Keep chapter 3 results-only, without causal explanation or literature comparison.
6. Keep chapter 4 parallel to chapter 3; compare with literature and explain findings there.
7. Keep conclusions matched to objectives only.

## Use References

- For thesis structure across designs, read `references/thesis-framework.md`.
- For prompts that direct an R/code agent to analyze chapter 3 from a proposal and dataset, read `references/r-chapter3-agent.md`.

## Design Guardrails

- Cross-sectional studies: say `associated with`, not causal `risk factor`, unless context permits.
- Case-control studies: report OR; do not estimate incidence directly.
- Cohort studies: define time zero, follow-up, censoring, outcomes, and loss to follow-up.
- Before-after studies: use paired/repeated analysis, not independent-group tests.
- Diagnostic studies: require index test, reference standard, 2x2 table, sensitivity, specificity, and ROC if applicable.
- Prognostic studies: ensure predictors are measured before outcomes.
- Survival analysis: require time-to-event and event indicator before using Kaplan-Meier or Cox.
- Genetic/SNP studies: report genotype/allele frequency, Hardy-Weinberg where appropriate, and genetic models only when justified.
- Prediction models: report discrimination, calibration, and validation when possible; warn about overfitting.
- Qualitative theses: use themes, codes, and quotes; do not force p-values.

## Chapter 3 Table Guardrails

- Follow the institution's caption and numbering format; local examples do not establish one mandatory punctuation style.
- Plan each table with a descriptive title, analytic N and percentage denominator, groups/timepoints, units, summary-statistic form, and test/model notes.
- For data cells, use one categorical layout per table (n (%) or clearly labeled n/% columns), state row/column bases, label units and summary forms, and keep counts/decimal precision consistent without imposing one decimal rule on every measure.
- Map each p-value to the applicable test and explain abbreviations, reference groups, and empty cells or dashes.
- Flag mismatches among the analysis plan, data, and table. Do not infer or silently change values, units, denominators, or tests.

## Output Pattern

When helping write or plan a thesis, produce:

1. Normalized title.
2. Study design classification.
3. Specific measurable objectives.
4. Opening/Đặt vấn đề outline.
5. Chapter 1 outline aligned to objectives.
6. Chapter 2 methods framework aligned to design.
7. Chapter 3 table/figure plan aligned to the objectives and design, with title, analytic N/denominator, groups/timepoints, cell layout, units/summary format, consistent number format, test/model notes, and model effect/CI/reference/scale/adjustment when applicable; use the 3.1 general-characteristics opening when appropriate.
8. Chapter 4 discussion outline parallel to chapter 3.
9. Conclusion and recommendation templates.
10. Risks, missing information, or design-analysis mismatches.

## R-Agent Prompt Pattern

When asked to create a prompt for an R agent, require the R agent to:

1. Read proposal/thesis draft first.
2. Extract title, objectives, chapter 2 variables, groups, timepoints, and statistical plan.
3. Read dataset and create a data dictionary.
4. Match proposal variables to dataset variables.
5. Create `analysis_plan_chapter3.md` before tables.
6. Write reproducible R scripts.
7. Generate Word/Excel tables and figures using the institution's format; include analytic N and percentage denominator (row/column base), group/timepoint headings, a consistent n (%) or n/% layout, units and summary-statistic form, integer counts and decimal precision appropriate to each measure, p-to-test notes, and model effect/estimate/95% CI/reference/increment/adjustment when relevant. Add Vietnamese chapter 3 comments and a run log.
8. Warn and stop specific analyses when required variables or design conditions are missing.
