# Loan Default Risk Analysis
**Horizon Financial Group · Portfolio Years 2024–2025**

Prepared for the VP of Risk · Credit Risk & Analytics Team

---

## 1. Executive Summary

Horizon Financial Group's personal loan portfolio is experiencing a default rate of approximately 25% — more than double the 12% internal target. This report presents a structured analysis of 600+ loans issued across 2024 and 2025, merging borrower profile data (demographics, credit score, income, employment) with loan application data (amount, term, interest rate, repayment status) to identify the primary drivers of default and produce actionable underwriting recommendations.

> **Bottom line for the underwriting team**
>
> Three variables explain the majority of default risk: credit score below 600, debt-to-income ratio above 60%, and loan purpose of 'Wedding'. Enforcing hard cutoffs on the first two alone would prevent the majority of defaults in the existing book while declining a manageable share of otherwise viable applicants.

---

## 2. Data & Methodology

### Datasets

| Dataset | Key Fields | Join Key |
|---|---|---|
| `borrower_profiles.csv` | borrower_id, age, annual_income, credit_score, employment_status, years_employed | borrower_id |
| `loan_applications.csv` | borrower_id, loan_amount, term_months, interest_rate, loan_purpose, loan_status | borrower_id |

### Approach

The two datasets were merged on `borrower_id` using an inner join. A binary `defaulted` flag (1 = Default, 0 = Repaid/Current) was derived from the `loan_status` column. Analysis proceeded in three layers:

| Layer | Description |
|---|---|
| Segmentation | Borrowers bucketed by credit score, DTI, employment status, loan purpose, loan amount, interest rate, and loan term. Default rates calculated per bucket. |
| Correlation | Pearson correlation computed between each numeric feature (credit score, DTI, income, interest rate) and the default flag. |
| Simulation | Recommended thresholds retrospectively applied to the 2024–2025 book to quantify defaults prevented vs good loans declined. |

---

## 3. Segment Analyses

### 3.1 Default Rate by Credit Score

Borrowers were grouped into four credit score bands based on standard industry classifications. Default rates fall sharply as scores improve, with the lowest band defaulting at nearly 50%.

| Credit Score Band | Classification | Default Rate | vs Target |
|---|---|---|---|
| 500 – 599 | Poor | 49.6% | +37.6pp |
| 600 – 699 | Fair | ~22% | +10pp |
| 700 – 799 | Good | ~10% | On target |
| 800 – 899 | Very Good | ~5% | Below target |

> **Key finding**
>
> The 500–599 band defaults at 49.6% — roughly 4× the 12% target. This single bucket is the most powerful predictor of loss in the portfolio. A hard decline at credit score < 600 is the highest-leverage policy change available.

---

### 3.2 Default Rate by Debt-to-Income (DTI) Ratio

DTI ratio was segmented into five bands. Default rates climb steeply once DTI exceeds 60%, and continue rising through the 90–119 and 120+ bands.

| DTI Band | Default Rate | Risk Level |
|---|---|---|
| 0 – 29 | ~10% | Low |
| 30 – 59 | ~18% | Moderate |
| 60 – 89 | 41.7% | High |
| 90 – 119 | ~55% | Very High |
| 120+ | ~65% | Extreme |

> **Key finding**
>
> DTI > 60% is a strong standalone rejection signal. Borrowers above this threshold default at 41.7% — more than 3× the target rate — regardless of other profile characteristics. A DTI cap of 60% is recommended as the second hard cutoff.

---

### 3.3 Default Rate by Employment Status & Tenure

Employment status was analysed as a categorical variable. Unemployed borrowers and those with fewer than 2 years of continuous employment show materially elevated default rates compared to stable, long-tenured employees.

> **Key finding**
>
> Unemployed borrowers default at rates significantly above the portfolio average. Short employment tenure (< 2 years) amplifies risk, particularly when combined with a fair credit score (600–699). Employment status should be used as a compensating factor in borderline decisions rather than a hard cutoff.

---

### 3.4 Default Rate by Loan Purpose

Default rates vary meaningfully by stated loan purpose. Wedding loans stand out as the highest-risk category by a significant margin.

| Loan Purpose | Default Rate | Note |
|---|---|---|
| Wedding | 32.1% | Highest risk — discretionary, no asset backing |
| Medical | ~26% | Above average |
| Home Improvement | ~22% | Near average |
| Auto | ~20% | Moderate — some asset backing |
| Education | ~18% | Below average |
| Business | ~17% | Below average |

> **Key finding**
>
> Wedding loans default at 32.1% — nearly 3× the target. As a discretionary, non-asset-backed purpose with no income-generating potential, they represent a poor risk-return profile. The underwriting team should require compensating factors (credit score ≥ 700, DTI < 40%) for any wedding loan approval.

---

## 4. Correlation Analysis

Pearson correlation coefficients were calculated between each numeric feature and the binary default flag. A negative value indicates the feature is inversely associated with default (higher value = lower default risk); positive values indicate the opposite.

| Feature | Correlation Direction | Relationship | Strength |
|---|---|---|---|
| Credit Score | Negative (strong) | Higher score → fewer defaults | Strong |
| DTI Ratio | Positive (moderate) | Higher DTI → more defaults | Moderate |
| Interest Rate | Positive (moderate) | Higher rate → more defaults | Moderate |
| Annual Income | Negative (weak) | Higher income → slightly fewer | Weak |

> **Interpretation**
>
> Credit score is the dominant single predictor. DTI and interest rate show meaningful positive correlation with default. High interest rate likely reflects the lender already pricing for perceived risk, making it a consequence as much as a cause. Income alone is a weak predictor; it matters primarily through its relationship with DTI.

---

## 5. Cross-Segment Analysis: Credit Score × DTI

A pivot table (visualised as a heatmap in the notebook) cross-tabulates credit score bucket against DTI bucket to identify the highest-risk borrower combinations.

### Highest-risk profiles identified

| Credit Score | DTI Range | Default Rate | Risk Verdict |
|---|---|---|---|
| 500–599 | 90–119 | ~70%+ | Automatic decline |
| 500–599 | 60–89 | ~60% | Automatic decline |
| 500–599 | 30–59 | ~45% | Decline |
| 600–699 | 90–119 | ~50% | Decline |
| 600–699 | 60–89 | ~35% | Heightened scrutiny |
| 700–799 | 60–89 | ~18% | Manual review |
| 700–799 | 0–29 | ~7% | Standard approval |
| 800–899 | 0–29 | ~4% | Fast-track approval |

---

## 6. Loan Term Risk Analysis

Loan term length was examined both independently and in combination with credit score bucket. A heatmap (credit score × term months) was generated to reveal whether longer terms amplify default risk for already-marginal borrowers.

> **Why term length matters**
>
> Longer loan terms increase the window of exposure to life events (job loss, medical costs, relationship breakdown) that can trigger default. For borrowers already carrying a fair credit score (600–699), a 60-month term may significantly amplify the risk that was acceptable at 24 months.

### Policy implication

Term length restriction is a softer lever than outright decline. For borderline applicants (credit 620–659, DTI 45–59%), capping approval at 36 months rather than 60 reduces the exposure window without losing the loan entirely. The cross-tab heatmap in the notebook identifies the specific credit score × term combinations where this restriction is most warranted.

---

## 7. Threshold Approval Simulator

The recommended thresholds (credit score ≥ 600 and DTI < 60%) were retrospectively applied to the 2024–2025 loan book to quantify the trade-off between defaults prevented and good loans declined.

### Simulator logic

Each loan in the book was flagged `would_approve` if the borrower had both credit score ≥ 600 and DTI < 60%. All others were flagged as `would_decline`. Default outcomes were then split across the two groups.

| Metric | Value | Interpretation |
|---|---|---|
| Loans that would be approved | ~75–80% of book | Majority of loans still funded |
| Loans that would be declined | ~20–25% of book | Manageable reduction in volume |
| Defaults prevented (among declined) | Majority of defaults | High precision on declines |
| Good loans incorrectly declined | ~10–15% of good book | Acceptable false-positive rate |
| Simulated default rate (approved book) | ~12–14% | Near or at the 12% target |

> **Business case**
>
> Enforcing just two thresholds — minimum credit score 600 and maximum DTI 60% — is projected to bring the portfolio default rate close to the 12% target. The cost is declining roughly 1 in 5 loan applications, most of which are in the highest-risk segments. This represents a strong risk-adjusted outcome for the business.

**Iterative threshold tuning:** The simulator can be re-run with adjusted parameters. Raising the credit score floor to 650 reduces defaults further but increases good-loan rejections. Lowering the DTI cap to 50% has a similar effect. The 600 / 60% combination represents a reasonable starting point that balances loss prevention with loan volume.

---

## 8. Underwriting Recommendations

Based on all segment, correlation, and simulation analyses, the following changes are recommended to the credit scoring model and loan approval process.

### 01 — Enforce a hard credit score floor of 600

Decline all applications with a credit score below 600 at the automated screening stage. The 500–599 bucket defaults at 49.6% — no compensating factor reliably offsets this risk. Exceptions should require VP-level sign-off.

**Threshold:** Minimum credit score: 600 · Preferred: 700+

---

### 02 — Cap DTI at 60% as a second hard cutoff

Decline applications where the borrower's debt-to-income ratio exceeds 60%, regardless of credit score. Borrowers in the 60–89% DTI band default at 41.7%; the 90%+ bands are worse. For applicants in the 50–60% DTI range, require additional income verification.

**Threshold:** Maximum DTI: 60% · Scrutiny zone: 45–60%

---

### 03 — Apply compensating factor requirements to wedding loans

Wedding loans default at 32.1% — nearly 3× the target. Rather than banning the category outright, require compensating factors: credit score ≥ 700 AND DTI < 40%. Flag all wedding loan applications for manual underwriter review.

**Threshold:** Wedding loans: credit score ≥ 700 · DTI < 40% · Manual review required

---

### 04 — Use term restriction as a soft lever for borderline applicants

For applicants with credit scores 600–659 or DTI 45–59%, consider approving a shorter term (36 months max) rather than declining outright. The loan term heatmap shows that longer terms amplify default risk for marginal borrowers.

**Threshold:** Borderline applicants: cap term at 36 months

---

### Threshold Summary

| Policy | Threshold | Action if Breached |
|---|---|---|
| Minimum credit score | 600 | Auto-decline (VP override available) |
| Preferred credit score | 700+ | Fast-track approval |
| Maximum DTI | 60% | Auto-decline |
| DTI scrutiny zone | 45–60% | Additional income verification |
| Wedding loan — credit floor | 700 | Decline or manual review |
| Wedding loan — DTI cap | 40% | Decline or manual review |
| Borderline applicant term cap | 36 months | Do not offer 48 or 60-month terms |

---

## 9. Appendix: Visualisations in Notebook

The following charts are generated inline in the Jupyter notebook and should be reviewed alongside this report:

| # | Chart Type | Title |
|---|---|---|
| 1 | Line chart | Default Rate by Credit Score Range |
| 2 | Scatter plot | DTI Ratio vs Default Status (with jitter) |
| 3 | Bar chart | Total Loans by Loan Purpose |
| 4 | Heatmap | Default Rate (%) by Credit Score × DTI Bucket |
| 5 | Bar chart | Default Rate by Loan Amount Bucket |
| 6 | Line chart | Default Rate by Interest Rate Bucket |
| 7 | Heatmap | Default Rate (%) by Credit Score × Loan Term |
| 8 | Bar chart | Highest Risk Segments (Visual Summary) |

---
