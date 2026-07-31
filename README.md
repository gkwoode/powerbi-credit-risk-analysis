# Nova Bank Credit Risk Analysis

A Power BI report analyzing consumer loan default risk for Nova Bank, built as a PBIP project (report + semantic model as source-controllable files).

## Repository structure

| Path | Contents |
|---|---|
| `CreditRiskAnalysis.pbip` | Entry point — open this in Power BI Desktop |
| `CreditRiskAnalysis.Report/` | Report definition (pages, visuals, theme) |
| `CreditRiskAnalysis.SemanticModel/` | Data model (tables, relationships, measures, Power Query) |
| `_backup_report/` | Prior version of the report, kept for reference |

Source `.docx` / `.pdf` / `.html` deliverables are excluded from version control (see `.gitignore`) — this repo tracks the PBIP source, not the exported writeups.

## Data model

Star schema fed by a single Power Query staging step (`CreditRiskSource`) that pulls a loan-level extract from SharePoint and derives surrogate keys:

- **Fact_CreditRisk** — one row per loan application (income, loan amount, interest rate, default flag, delinquencies, utilization, DTI/LTI, etc.), plus calculated risk bands and segments.
- **Dim_Loan** — distinct loan intent / grade / term combinations.
- **Dim_BorrowerProfile** — junk dimension of borrower demographics (gender, marital status, education, employment type, home ownership).
- **Dim_Geography** — country / state / city.
- **Dim_AgeBand** — borrower age grouped into 10-year bands.
- **Recovery Rate** — hidden what-if parameter table (0–100%, default 50%) driving `Expected Loss`.
- **Measure Table** — all DAX measures, organized into `Volume`, `Borrower`, and `Risk` display folders.

### Key measures

- `Default Rate (%)` / `Default Rate by Amount (%)` — defaults as a share of loan count / principal
- `High Risk Borrowers` / `High Risk (%)` — via the `High Risk Borrower` flag (DTI > 40%, utilization > 70%, or any delinquency)
- `Expected Loss` — defaulted principal net of recoveries, driven by the Recovery Rate parameter
- `Risk Segment` — affordability-based segmentation (loan-to-income / debt-to-income) used to size the highest-risk slice of the book

## Report pages

1. **Overview**
2. **Borrower Profile Analysis**
3. **Affordability & Leverage**
4. **Loan Characteristics & Risk**
5. **Credit History**
6. **Executive Risk Summary**

## Getting started

Open `CreditRiskAnalysis.pbip` in Power BI Desktop. The semantic model refreshes from a SharePoint-hosted Excel extract (`CreditRiskSource` in Power Query) — you'll need access to the source site, or you can point the query at a local copy of the dataset.
