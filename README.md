# ICU Mortality: Interpretable Inference Meets Predictive Power

![R](https://img.shields.io/badge/R-4.5-276DC3?logo=r&logoColor=white)
![Models](https://img.shields.io/badge/models-logistic%20%2B%20LightGBM-1F7A63)
![CV%20AUC](https://img.shields.io/badge/LightGBM%20CV%20AUC-0.896-15433C)
![Status](https://img.shields.io/badge/status-complete-success)

A two-track analysis of in-hospital mortality across **91,713 ICU admissions** (WiDS Datathon 2020 / GOSSIS): an **interpretable logistic regression** for clinical inference, and a cross-validated **LightGBM** model for predictive performance — reported with an honest accounting of why the two AUCs differ.

📊 **[Read the full project write-up →](https://jonathanmuniz13.github.io/PORTFOLIO-REPO/projects/icu-mortality/)**

---

## Key results

| Model | AUC | What kind of number |
|---|---|---|
| Logistic regression | 0.784 | Apparent (in-sample) — optimistic |
| LightGBM | **0.896** | 5-fold cross-validated — realistic |

The cross-validated LightGBM genuinely outperforms, but the logistic 0.784 is in-sample and therefore inflated — so the *real* gap is smaller than a naive comparison suggests. Reporting that distinction (rather than the bigger number alone) is the point. Note: LightGBM's top features are APACHE's own pre-computed mortality probabilities, so part of the 0.896 reflects recovering an existing clinical risk score embedded in the data.

## Methods

- **Logistic regression** (10 interpretable predictors): missingness-based variable selection, log-transformed pre-ICU LOS, multicollinearity checked via correlation heatmap + VIF, odds ratios scaled by IQR for interpretability
- **Complete-case selection-bias check** (retained vs. dropped patients) before trusting the model
- **LightGBM** (72 features): `scale_pos_weight` for class imbalance, **5-fold stratified cross-validation**, refit for a Kaggle submission pipeline
- **Linear probability model** as a sensitivity check on the logistic findings
- Diagnostics (ROC, decile calibration, predicted-probability density) reported as **apparent / in-sample** except the cross-validated LightGBM AUC

## ⚠️ Data — not included in this repo

This project uses the **WiDS Datathon 2020 (GOSSIS)** dataset, which is **not committed** here (size + competition terms).

1. Download `training_v2.csv` (and `unlabeled.csv` for the submission step) from the [WiDS Datathon 2020 Kaggle page](https://www.kaggle.com/c/widsdatathon2020).
2. Place the file(s) in the repo root (or a `data/` folder, matching the path in the `.Rmd`).
3. Then render the analysis (below).

A `.gitignore` excludes `*.csv` so the raw data is never accidentally committed.

## Repository structure

```
.
├── icu_mortality.Rmd        # full analysis source
├── Wids-ICU-Kaggle.pdf      # rendered report
├── .gitignore               # excludes raw CSVs
└── README.md
```

## How to run

Requires R (≥ 4.5). Key packages:

```r
install.packages(c(
  "tidyverse", "skimr", "broom", "pROC", "car",
  "ggcorrplot", "scales", "lightgbm", "caret"
))

rmarkdown::render("icu_mortality.Rmd")
```

> `lightgbm` may require a system build toolchain — see the [LightGBM R install guide](https://github.com/microsoft/LightGBM/tree/master/R-package) if installation fails.

## Tools

`R` · `tidyverse` · `LightGBM` · `pROC` · `car` · `broom` · `ggplot2` · `caret`

## Author

**Jonathan Muniz** — [Portfolio](https://jonathanmuniz13.github.io/PORTFOLIO-REPO/) · [LinkedIn](https://www.linkedin.com/in/jonathan-muniz-fl) · [GitHub](https://github.com/Jonathanmuniz13)
