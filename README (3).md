# Can This Income Model Be Trusted? A Model Validation Report

Most data science projects stop at "my model scores 0.91 AUC." This one asks the harder question: **should anyone actually use it?**

I validated an income classifier (UCI Adult census data — predict whether annual income exceeds $50K) the way a bank's Model Risk Management team would, following the Federal Reserve's **SR 11-7** guidance. The model performs well. The validation says: *not so fast.*

## The headline finding

The model is accurate — and unfair. Women are selected at less than one-tenth the rate of men:

- **Disparate impact ratio (Female/Male): 0.327** — far below the 0.8 four-fifths guideline
- The two groups don't just differ in selection rate — they **fail in different ways**: Male FPR 0.105 vs Female 0.023
- By race: Black selection rate 0.107 vs White 0.222

This is the difference between *modeling* and *model validation*: the model learned a biased target (income reflects historical labor-market disparities), and "accurate" predictions can still be unfair. The report documents causes, limitations, and mitigations — not just the gap.

## What the validation covers

| SR 11-7 dimension | Verdict |
|---|---|
| Conceptual soundness | **Medium** — standard classifiers, but the target bakes historical bias into the objective |
| Data quality | **Medium** — 5.6% missingness handled explicitly; 1994 census data is dated |
| Ongoing monitoring need | **High** — material fairness flags demand continuous group-metric tracking |

**Overall: Medium-High risk. Deploy only with conditions** — fairness monitoring, per-group thresholds, human review of high-stakes decisions. The notebook ends with a 5-sentence brief written for a non-technical manager.

## Performance (for the technically curious)

Random Forest vs logistic regression baseline on a stratified 80/20 split:

- Random Forest: accuracy 0.857, **AUC 0.908**, F1 0.682
- Stability: AUC bootstrap 95% CI **[0.900, 0.917]** — stable under resampling
- Not uniform across age: under-30 AUC 0.928 vs 0.870–0.881 for older bands (flagged for monitoring)

## Files

- `01-model-validation-framework.ipynb` — the full report: code, results, and conclusions in 20 cells
- `fairness_by_sex.csv` / `fairness_by_race.csv` — the group-metric evidence tables
- `README.md` — this page

## Reproduce

Open the notebook in [Google Colab](https://colab.research.google.com), upload `adult.data` to the file panel, run all cells. (Local: `pip install pandas scikit-learn jupyter`.)

## About

Built as a hands-on validation project by a UCSD Data Science master's student (June 2027), working toward model risk / AI governance roles. Validation mindset: a model is guilty until proven innocent.
