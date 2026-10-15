# R Chapter 3 Agent Prompt Reference

Use this reference when creating prompts for an R/code agent that receives a proposal/thesis draft and dataset, then analyzes and presents chapter 3.

## Required Inputs

```text
Proposal/thesis: [path]
Dataset: [path]
Excel sheet if relevant: [sheet]
Output directory: [path]
Language: Vietnamese
```

Optional but useful:

```text
ID variable: [id]
Main grouping variable: [group]
Main outcome: [outcome]
Timepoints: [T0, T1, T2]
Significance threshold: p < 0.05
Preferred outputs: docx/xlsx/png/md
```

## Mandatory Agent Workflow

1. Read proposal/thesis first.
2. Extract title, objectives, chapter 2, variables, groups, timepoints, outcomes, and statistical plan.
3. Read dataset and create data dictionary.
4. Match proposal variables to dataset variables.
5. Create `qc_missing.xlsx` or equivalent variable-match report.
6. Create `analysis_plan_chapter3.md` before tables.
7. Write reproducible R scripts.
8. Generate chapter 3 tables/figures.
9. Generate Vietnamese result comments for each table/figure.
10. Generate `run_log.txt` with warnings and output paths.

## Chapter 3 Organization and Presentation Rules

The reviewed local corpus has 29 result chapters; all 29 extracted sections begin with 3.1 on general characteristics. Treat this as a local default when appropriate, not an official institutional requirement.

- Order later sections by the approved objectives, study design, and analyses performed. Treat design-specific table lists below as starting points, not a fixed sequence.
- Apply the Table Presentation Rules below as quality guidance; they do not replace the institution's required template.
- Keep result comments concise, refer to the correct table or figure, and check that the text matches its groups, denominator, time point, and values. Do not copy results or conclusions from example theses.
- Text extraction did not verify chart images; do not assume that a source figure's axes, legend, readability, or correctness have been checked.

## Table Presentation Rules

- Use tables for exact values, multi-variable or multi-group comparisons, and model estimates. Avoid duplicating the same values in a figure unless the figure adds a distinct visual purpose.
- Keep the institution's required title punctuation and placement. Give tables and figures separate sequential numbering; check that every caption and in-text reference is unique and correct.
- Put the variable/outcome and relevant population, group, or timepoint in the title. The title must agree with the displayed rows and columns.
- Use the Data values subsection below for in-cell layouts, analytic N/denominator, units, summary-statistic form, numeric precision, and missing/blank markers.
- Map each reported p-value to its test. Use one note if the whole table uses one test; if tests vary, mark each relevant p-value and define the markers. Do not select or change a test merely to fit a table template; verify it against the design, analysis plan, variable type, group count, and paired/repeated structure.
- For regression or diagnostic-performance tables, report the measure (such as OR, HR, β, or AUC), 95% CI, p-value, reference group, unit of increase, and adjustment set as applicable. Include units and direction for cutoffs.
- Check that captions, denominators, units, footnotes, p-to-test mappings, and narrative comments agree with scripted outputs. Flag unresolved source/data conflicts; do not silently alter a result.

### Trình bày dữ liệu trong thân bảng

- Lay out rows as variables/categories and columns as comparison groups or timepoints. Put the actual N in each relevant header when sample sizes differ.
- Use one categorical layout per table: n (%) in one cell or clearly labeled n and % columns. Do not report percentages alone when the numerator matters; state the row/column denominator and note multiple-response items.
- Put units in variable labels and the summary form in column headers. Use mean ± SD, median (Q1–Q3), or median (IQR) according to the analysis plan; never leave parenthetical values unexplained.
- Display counts as integers. Follow the institution's decimal separator; keep precision consistent for the same measure across groups/timepoints, but do not force every variable to use the same number of decimals.
- Apply rounding and numeric formatting in R. Keep source values unchanged; do not manually adjust cells to make totals appear exact. Distinguish real zero, missing, unavailable, not analyzed, not estimable, reference, blank, and dash.

## Suggested Output Structure

```text
output/
  data_dictionary.xlsx
  qc_missing.xlsx
  analysis_plan_chapter3.md
  scripts/
    00_setup.R
    01_import_clean.R
    02_derive_variables.R
    03_tables_chapter3.R
    04_figures_chapter3.R
    05_models_chapter3.R
    99_run_all.R
  tables_word/
    Bang_3_1.docx
  tables_excel/
    Bang_3_1.xlsx
  figures/
    Bieu_do_3_1.png
  chapter3_results.md
  chapter3_nhan_xet.md
  run_log.txt
```

For small projects, one `analysis_chapter3.R` file is acceptable if it has clear sections.

## General Prompt Template

```text
You are an R analysis agent for chapter 3 of a Vietnamese medical thesis.

Inputs:
- Proposal/thesis draft: "[PATH_DE_CUONG]"
- Dataset: "[PATH_DATA]"
- Sheet: "[SHEET_OR_AUTO]"
- Output directory: "[PATH_OUTPUT]"

Tasks:
1. Read the proposal first. Extract title, specific objectives, chapter 2 methods, variables, inclusion/exclusion criteria, design, groups, timepoints, outcomes, and statistical plan.
2. Read the dataset. Create a data dictionary: variable name, detected type, missing count, missing percent, unique values, min/max or levels.
3. Match proposal variables to dataset variables. Create `qc_missing.xlsx` with required variable, found variable, missing variable, likely matching variable, and notes.
4. Create `analysis_plan_chapter3.md` listing each planned table/figure: number, Vietnamese title, objective served, groups/timepoints, analytic N and denominator, variables/units, summary form, test/model, relevant notes, and output file.
5. Write reproducible R code. Do not edit results manually. All cleaning, recoding, grouping, labels, and missing-data handling must be scripted.
6. Generate Vietnamese chapter 3 tables in the institution's format. For each table, use unique numbering and a content-matched title; report analytic N by group/timepoint, percentage denominator and row/column base, units, and the exact summary-statistic form; use one consistent n (%) or n/% column layout; display counts as integers and apply the institution's decimal convention with precision consistent per measure; map each p-value to its test; report model effect measure and estimate (OR/HR/PR/β/AUC), 95% CI, p, reference group, unit increase for continuous predictors, and adjustment set when applicable; explain blanks/dashes and check against scripted output.
7. Generate `chapter3_nhan_xet.md` with 1-3 Vietnamese result sentences per table/figure. Describe results only; do not discuss causes or literature.
8. Generate `run_log.txt` with packages, dataset rows/columns, final N per analysis, warnings, and output paths.

Statistical rules:
- Categorical variables: n and %.
- Normal continuous variables: mean ± SD.
- Non-normal continuous variables: median (Q1-Q3).
- Normality: use skewness/kurtosis or a documented rule; be consistent.
- Two independent normal groups: t-test.
- Two independent non-normal groups: Mann-Whitney U.
- Three or more normal groups: ANOVA.
- Three or more non-normal groups: Kruskal-Wallis.
- Categorical variables: Chi-square or Fisher exact when expected counts are low.
- Before-after paired data: paired t-test, Wilcoxon signed-rank, or McNemar.
- Repeated measures: repeated-measures ANOVA, Friedman, mixed model, or GEE as appropriate.
- Binary outcome: logistic regression; report OR, 95% CI, p.
- Sparse events/separation: consider Firth logistic and warn.
- Cross-sectional common outcome: consider robust Poisson for PR if appropriate.
- Time-to-event: Kaplan-Meier/log-rank/Cox only if time and event variables exist.
- ROC: only if there is a clear binary outcome and predictor; report AUC, 95% CI, Youden cutoff, sensitivity, specificity.

Do not:
- Analyze beyond objectives unless labeled exploratory.
- Use Cox without time-to-event data.
- Use ROC without binary outcome.
- Analyze before-after as independent groups.
- Overfit multivariable models with too few events.
- Interpret association as causation for observational/cross-sectional designs.
- Create outcomes or grouped variables not defined in proposal unless flagged for confirmation.
```

## Design-Specific Additions

### Descriptive/Cross-Sectional

Request tables:

- Bảng 3.1: demographic/baseline characteristics.
- Bảng 3.2: clinical characteristics.
- Bảng 3.3: laboratory/paraclinical characteristics.
- Bảng 3.4: main variable/prevalence/severity distribution.
- Later tables: associations and regression if objectives require.

### Treatment/Before-After

Require:

- Identify intervention/regimen.
- Identify assessment timepoints.
- Define response, target achievement, failure, adverse events.
- Use paired/repeated analysis.

Tables:

- Baseline characteristics.
- Baseline disease/main variable.
- Change after treatment or over time.
- Response/target achievement.
- Adverse events.
- Factors associated with response.

### Prognostic/Diagnostic/ROC

Require:

- Predictor variable.
- Binary outcome or time-to-event outcome.
- Predictor measurement time.
- Outcome ascertainment time.
- Clinical adjustment variables.

Tables/figures:

- Baseline characteristics.
- Predictor distribution and outcome rate.
- Comparison by outcome status.
- Regression model.
- ROC/AUC table.
- ROC figure.
- Kaplan-Meier only if time-to-event exists.

### Genetics/SNP

Require:

- SNP/gene name.
- Reference allele and risk allele.
- Possible genotypes.
- Genetic models: codominant, dominant, recessive, additive, allelic.
- Related outcome or clinical variable.

Tables:

- Baseline characteristics.
- Genotype and allele frequencies.
- Hardy-Weinberg equilibrium if appropriate.
- Clinical/lab features by genotype.
- Genetic model associations.
- Adjusted regression if data supports it.

Warn if genotype is rare, cells are zero, events are few, or confidence intervals are wide.

### Dirty Data/QC

Add this instruction when dataset quality is unknown:

```text
Before chapter 3 analysis, perform controlled data QC:
1. Do not delete rows without documented reason.
2. Create missingness summary for all variables.
3. Create outlier summary for continuous variables: min, Q1, median, mean, Q3, max, implausible values.
4. Create levels summary for categorical variables: levels, counts, suspected typos.
5. Keep original columns and create `_clean` variables for recoded versions.
6. Any grouping decision must come from proposal or be flagged for confirmation.
```

## Table Formatting Rules

Descriptive categorical table:

```text
| Characteristic | n | % |
```

Continuous table:

```text
| Variable | Mean ± SD |
| Variable | Median (Q1-Q3) |
```

Group comparison:

```text
| Characteristic | Group 1 | Group 2 | p |
```

Regression:

```text
| Factor | Crude OR/HR/PR | 95% CI | p | Adjusted OR/HR/PR | 95% CI | p |
```

ROC:

```text
| Predictor | AUC | 95% CI | Cutoff | Sensitivity (%) | Specificity (%) | p |
```

## Vietnamese Result Comment Rules

- Write 1-3 sentences per table.
- State the most important result first.
- State p/OR/HR/AUC when relevant.
- Do not repeat every row.
- Do not compare with literature.
- Do not explain mechanisms in chapter 3.
- Avoid causal language unless design supports it.

Example:

```text
Bảng 3.1 cho thấy nhóm tuổi thường gặp nhất là ..., chiếm ...%. Tỷ lệ nam và nữ lần lượt là ...% và ...%.

Bảng 3.5 ghi nhận tỷ lệ đạt mục tiêu điều trị ở nhóm ... cao hơn nhóm ..., sự khác biệt có ý nghĩa thống kê với p = ... .

Bảng 3.8 cho thấy sau hiệu chỉnh, ... vẫn liên quan với kết cục ..., OR hiệu chỉnh = ...; KTC 95%: ...; p = ... .
```

## R Package Hints

Use installed packages when available; do not add dependencies unnecessarily.

- Import Excel: `readxl`, `openxlsx`.
- Cleaning: `dplyr`, `tidyr`, `stringr`, `janitor`.
- Tables: `gtsummary`, `flextable`, `officer`.
- Figures: `ggplot2`.
- ROC: `pROC`.
- Firth logistic: `logistf` or `brglm2`.
- Survival: `survival`, `survminer`.
- Labels: `labelled`.

## Reproducibility Rules

- One command should rerun the full analysis.
- Prefer relative paths inside project output.
- Set seed if randomness is used.
- Do not manually edit Excel/Word after export.
- Never delete the raw dataset.
- Log all warnings to `run_log.txt`.
