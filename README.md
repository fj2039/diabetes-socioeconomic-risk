# Socioeconomic Patterns in Diabetes Risk

**Principles of Data Science II · NYU · Fall 2025**

Daniella Urdaneta · Fatema Jaynab

---

## The question

Diabetes is usually framed as a medical condition. We wanted to know how much of it tracks with income, education, and whether you can afford to see a doctor.

So we asked two things. Do socioeconomic conditions predict diabetes at the population level? And can you build a usable classifier from socioeconomic features alone?

The answers turned out to be different, and the gap between them is the interesting part.

## Data

253,680 records, 22 variables spanning medical, behavioural and demographic information. No missing values, all integer-coded. 13.9% of individuals have diabetes, 86.1% do not.

That imbalance matters more than it first appears.

## Population-level findings

**Income tracks diabetes strongly and almost monotonically.**

| Income bracket | Diabetes rate |
|---|---|
| 1 (lowest) | 24.3% |
| 2 | **26.2%** |
| 3 | 22.3% |
| 4 | 20.1% |
| 5 | 17.4% |
| 6 | 14.5% |
| 7 | 12.2% |
| 8 (highest) | **8.0%** |

Bracket 2 rather than bracket 1 holds the peak, so the relationship isn't perfectly linear at the bottom. From bracket 2 onward the decline is steady, ending at roughly a third of the rate.

**Education follows the same shape.**

| Education bracket | Diabetes rate |
|---|---|
| 1 | 27.0% |
| 2 | **29.3%** |
| 3 | 24.2% |
| 4 | 17.6% |
| 5 | 14.8% |
| 6 (highest) | **9.7%** |

Again the peak sits at bracket 2, not bracket 1. Worth a closer look than we gave it.

**BMI looks like the mechanism.** Mean BMI falls with income in step with the diabetes rate, from 29.76 in bracket 2 to 27.57 in bracket 8. Diabetics and non-diabetics share a similar modal BMI, but the diabetic distribution is right-skewed. So lower income is associated with higher BMI, and higher BMI with diabetes. Socioeconomic status appears to act on diabetes risk partly *through* body weight rather than directly.

**Income and education compound.** The heatmap of the two together shows the highest rates where both are low and the lowest where both are high, with a clean gradient between. These are not independent effects stacking; they concentrate.

**Insurance is not access.** The most striking cell in the affordability analysis: people who *have* healthcare coverage but cannot afford a doctor show the **highest** diabetes prevalence of any group. People with *no* coverage who can still afford a doctor show the **lowest**. Coverage without affordability is worth less than affordability without coverage.

## Where it breaks down

We built two logistic regression models to see whether population-level signal converts into individual-level prediction.

| | Socioeconomic only | Full features |
|---|---|---|
| Accuracy | 0.860 | 0.862 |
| Precision (diabetes) | 0.41 | 0.52 |
| **Recall (diabetes)** | **0.00** | **0.16** |
| F1 (diabetes) | 0.01 | 0.24 |

**Both models report 86% accuracy. Both are close to useless, and the first one is entirely useless.**

The socioeconomic-only model found 24 diabetics out of 7,069. It missed 7,045 of them. Its 86% accuracy comes almost entirely from correctly identifying non-diabetics, which is what you get for free by predicting "no diabetes" every time on an 86/14 split.

Adding health and lifestyle features raised true positives from 24 to 1,122 and cut false negatives from 7,045 to 5,947. Better. Still missing 84% of diabetics.

**The coefficients show why.** In the socioeconomic-only model the strongest predictors are biological sex, age, and barriers to healthcare access. Add clinical variables and they take over completely: high blood pressure, high cholesterol, poor self-reported health, elevated BMI. The socioeconomic terms shrink.

Which is consistent with the BMI finding. Income and education don't act on diabetes directly, they act through health conditions and behaviours. Once you measure those directly, the socioeconomic proxies have little left to explain.

## What we take from it

Socioeconomic data describes populations well and individuals badly. Knowing someone's income bracket tells you something real about the rate in their group, and close to nothing about whether that particular person has diabetes.

That distinction is easy to lose when a model reports 86% accuracy.

The policy implication survives the modelling failure. The gradients are real: health education in lower-income and lower-education communities, affordability rather than coverage alone, community programmes targeting BMI, and reducing the financial and informational barriers that keep preventive care out of reach.

## Repository layout

```
notebook/
  diabetes_socioeconomic_analysis.ipynb   EDA, heatmaps, both models
docs/
  Socioeconomic Patterns in Diabetes Risk Report.pdf
  Project Proposal.pdf
```

The notebook is committed with its outputs, so all 10 figures and the model results read on GitHub without running anything.

## Data access

`diabetes.csv` is not committed. It was provided in Principles of Data Science II by Pascal Wallisch, and is a diabetes health indicators dataset derived from CDC BRFSS survey data. Put it in the working directory to re-run the notebook.

## Tools

pandas, seaborn, matplotlib, scikit-learn

## Contributions

Fatema wrote the code: the analysis notebook, the EDA and visualizations, and both logistic regression models. Daniella wrote the report.
