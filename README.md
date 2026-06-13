# Horizon Financial Group — Loan Default Risk Analysis

## Overview

Analysis of 600+ personal loans issued in 2024–2025 to identify drivers of a ~25% default rate — more than double the company's 12% target. Findings directly inform updates to the credit scoring model and approval thresholds.

## Files

| File | Description |
|---|---|
| `horizon_financial_group.ipynb` | Full analysis notebook — all code, charts, and findings |
| `analysis_report.pdf` | Standalone report for stakeholders and the underwriting team |
| `data/borrower_profiles.csv` | Borrower demographics, income, credit score, employment |
| `data/loan_applications.csv` | Loan amount, term, interest rate, repayment status |

## Key Findings

| Risk Factor | Default Rate |
|---|---|
| Credit score < 600 | 49.6% |
| DTI > 60% | 41.7% |
| Wedding loan purpose | 32.1% |
| Portfolio average | ~25% |
| Company target | 12% |

## Recommended Thresholds

- **Minimum credit score:** 600 (hard decline below this)
- **Maximum DTI:** 60%
- **Preferred credit score:** 700+

## Running the Notebook

1. Place `borrower_profiles.csv` and `loan_applications.csv` in a `data/` folder (or update paths in the first cell).
2. Install dependencies: `pip install pandas numpy matplotlib seaborn`
3. Run all cells top to bottom.

## Dependencies

```
pandas · numpy · matplotlib · seaborn
```

