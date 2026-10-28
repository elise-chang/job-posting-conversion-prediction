# Predicting Low-Performing Job Postings with Machine Learning

**Machine learning case study · UC Berkeley Data 144**  

## Overview

Can job-posting characteristics predict which listings will receive relatively few applications compared with their views? We investigated this question using LinkedIn job-posting data from 2023–2024. The goal was to identify patterns associated with **low application-to-view conversion** and explore how such signals might inform hiring-platform design and recruiter analysis.

This is a collaborative course research project, not a deployed prediction service. The notebook and original presentation are included as a record of the work.

## Data and methodology

- **Source:** [LinkedIn Job Postings (2023–2024), Arsh Koneru and contributors, Kaggle](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings) (dataset page lists **CC BY-SA 4.0**).
- **Initial scope:** **123,849 job postings** before filtering.
- **Modeling sample:** approximately **23,085 postings** after excluding rows with unavailable/invalid application or view counts and extreme conversion-rate outliers; **4,617** in the held-out test set.
- **Target:** `applications / views`; bottom quartile of the filtered data labeled low-conversion.
- **Features:** salary and company attributes, location indicator, missing-value indicators, categorical encoding, text-derived length features, and TF-IDF representations of job descriptions/titles/skills.
- **Models:** decision tree, random forest, XGBoost, and logistic regression with TF-IDF text features.

### Results

| Model | Test ROC-AUC |
|---|---:|
| Decision tree | 0.550 |
| Random forest | 0.649 |
| XGBoost | 0.631 |
| **Logistic regression + TF-IDF** | **0.683** |

Logistic regression performed best on ROC-AUC in the original run, with a **0.46 F1-score** for the low-conversion class (**0.36 precision**, **0.64 recall**). These are modest predictive results; they do not establish production readiness. In particular, the reported logistic regression model emitted a convergence warning.

**Comparison caveat:** The tree models used structured features, while logistic regression also used TF-IDF text, so the table compares **model/feature configurations**, not algorithms holding inputs constant.

## Repository contents

- [`analysis.ipynb`](analysis.ipynb) — code, retained original model outputs, charts, and a narrative walkthrough.
- [`presentation.pptx`](presentation.pptx) — original group presentation.
- [`requirements.txt`](requirements.txt) — Python dependencies.
- [`data/README.md`](data/README.md) — instructions to obtain the external data files.

## Running the notebook

1. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/arshkon/linkedin-job-postings). Place the relevant CSVs in `data/`, using the filenames `postings.csv`, `companies.csv`, and `employee_counts.csv` (rename or copy files from their downloaded subdirectories if necessary).
2. Install the packages:

   ```bash
   python -m pip install -r requirements.txt
   ```

3. Open `analysis.ipynb` from the repository root and run the cells in order.

The exported notebook retains original Deepnote results for review. **The cleaned export has not been rerun** against a local dataset. Reproducing results may require matching the original package versions and checking the dataset release/version.

## Interpretation and limitations

- The conversion target requires views and applications, which are missing for many postings; the effective analysis sample is substantially smaller than the full dataset.
- The positive class comprises roughly one-quarter of the filtered sample. A majority-class accuracy baseline is approximately **75%**, so accuracy alone would be misleading.
- Logistic regression generated a convergence warning; coefficient magnitudes should be treated cautiously.
- Company information was joined using the latest available employee snapshot, not necessarily the snapshot at the time each posting was published.
- A random posting-level split can place posts from the same company in both train and test, and the low-conversion threshold was defined before splitting. A more rigorous follow-up would use temporally valid/grouped evaluations.
- The results are **associational**, not causal: a feature associated with lower conversion does not establish that changing that feature will improve applications.
- Data covers 2023–2024 and may not generalize to later hiring markets.

### Possible extensions

Time-aware validation, stronger baselines, class-specific error analysis, threshold selection, calibrated predictions, hyperparameter tuning, convergence checks, and controlled comparison of text representations.
