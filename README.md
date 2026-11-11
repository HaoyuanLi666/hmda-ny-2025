# Mortgage Denial Patterns in New York, 2025

Haoyuan Li 
Repository: https://github.com/HaoyuanLi666/hmda-ny-2025

## Context

This is an independent project analyzing public mortgage lending data. It uses only public government data and is not drawn from any employer's or advisor's work.

## Question

Among home-purchase mortgage applications in New York in 2025, how do denial rates differ across lenders, loan types and applicant groups, and which differences remain after accounting for applicants' basic financial profile?

## Data

Public 2025 Home Mortgage Disclosure Act (HMDA) records for New York State from the FFIEC HMDA Data Browser. I filtered 430,554 records to 113,145 first-lien, owner-occupied, 1 to 4 unit home-purchase applications that were either originated or denied, checking every code against the official data dictionary and reporting row counts after each step.

## Methods

- **Cleaning and filtering** in pandas, with code checks and row counts at each step, and explicit handling of missing and exempt values
- **Descriptive analysis** of denial rates by loan type and by income relative to the area median, with confidence intervals
- **Lender comparison** using indirect standardization: each lender's observed denials are compared with the denials expected from its mix of applications (loan type, income, debt-to-income and loan-to-value bands)
- **Applicant group comparison**: denial gaps relative to White applicants, recomputed among applicants with the same financial profile as controls are added one at a time
- **SQL** in SQLite, using common table expressions, window functions and UNION ALL, including a check that reproduces the pandas results
- **Visualizations** in matplotlib

## Key findings

1. The overall denial rate was 11.8%. Debt-to-income ratio, collateral and credit history were the most common denial reasons, together accounting for 72% of denials.
2. Denial rates fell from 32.5% for applicants earning under half the area median income to 7.7% for those earning over twice the median. FHA applications had the highest denial rate (15.9%, against 11.3% for conventional and 11.0% for VA), and unlike conventional rates, FHA rates did not keep falling at higher incomes.
3. Of the 151 lenders with at least 100 applications, 32 denied significantly more applications and 40 significantly fewer than their applicant mix predicts (|z| > 3). Several lenders that rank highest in Section 3 also have the highest denial rates within their loan type in the SQL analysis.
4. Black and Hispanic applicants were denied at 19.2% and 16.7%, compared with 10.1% for White applicants. Among applicants with the same income, debt-to-income ratio, loan-to-value ratio and loan type, the gaps narrowed from 9.2 to 6.3 percentage points for Black applicants and from 6.7 to 4.9 points for Hispanic applicants.

## Limitations

Public HMDA data does not include credit scores, full credit history or cash reserves, so gaps that remain after these controls are signals worth further investigation, not proof of discrimination. The sample excludes withdrawn and incomplete applications, so lenders that screen out applicants before a decision may appear to have low denial rates. Lenders with fewer than 100 applications were excluded from lender rankings because their rates are too noisy.

## Relevance

This project reflects how I would approach OAG investigations: acquiring public data, testing it for reliability, quantifying patterns, and communicating findings together with their limitations.

## How to reproduce

1. Download the data from the FFIEC HMDA Data Browser (https://ffiec.cfpb.gov/data-browser/): year 2025, state New York, all records. Save the CSV in `data/`. The raw file is not included in this repository.
2. Install Python 3 with pandas and matplotlib (`sqlite3` is part of the standard library).
3. From the `notebooks/` folder, open `hmda_ny_2025_analysis.ipynb` and run all cells.

```
hmda-ny-2025/
  data/        raw CSV and generated files (not committed)
  figures/     charts saved by the notebook
  notebooks/   hmda_ny_2025_analysis.ipynb
```
