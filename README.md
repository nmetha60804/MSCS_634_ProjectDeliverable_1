# Deliverable 1: Data Collection, Cleaning, and Exploration

This deliverable uses the [Healthcare Dataset on Kaggle](https://www.kaggle.com/datasets/prasad22/healthcare-dataset), a synthetic dataset of hospital admissions. The notebook documents the dataset, inspects and cleans the data, and explores its distributions and relationships.

## Getting started

1. Download the CSV from Kaggle.
2. Save it as `healthcare_dataset.csv` in this folder's `data/` directory.
3. Open and run `Deliverable_1_EDA.ipynb` from top to bottom.

The notebook also checks the common workspace-root location `deliverable_1/data/healthcare_dataset.csv`.

## Cleaning and exploration completed

- The source CSV contains 55,500 rows and 15 columns, exceeding the assignment's minimum size. It has no missing values and 534 exact duplicate rows.
- Standardized column names, trimmed and collapsed whitespace in text fields, converted age and billing amount to numeric, parsed admission/discharge dates, and derived length of stay. Exact duplicate rows were removed; no invalid ages, unparseable dates, or negative stays were found.
- Category labels are preserved in their canonical spelling; for example, `AB+` and `UnitedHealthcare` are not changed by title-casing.
- No imputation was necessary because the inspected fields have no missing values. IQR review flags no outliers in age, billing amount, or length of stay. Separately, 108 raw billing amounts are negative, with 106 remaining after duplicate removal. These are retained for review because the dataset does not explain whether negative bills represent errors or adjustments; IQR alone cannot determine that.
- Excluded name, doctor, hospital, and room number from the analytical frame. The cleaned analytical CSV contains 54,966 rows and 12 columns.
- Test results are nearly balanced: 33.5% Abnormal, 33.3% Normal, and 33.1% Inconclusive.
- Numeric correlations are near zero; the largest absolute pairwise correlation among age, billing amount, and length of stay is about 0.008. The EDA shows no strong linear relationships, so later predictive models should be compared with simple baselines.
- Median billing varies by medical condition (about $24,911 for Cancer to $26,146 for Obesity), but this is descriptive only and not strong evidence of a meaningful difference. The dataset is synthetic.
- Six visualizations (numeric distributions, category counts, billing by condition, and a numeric correlation heatmap) display inline in the notebook; no separate figures folder is used.

## Challenges and decisions

- The dataset is synthetic, so findings cannot be generalized to real patients or healthcare systems.
- Negative billing values have no explanatory metadata. They are reported separately from statistical outlier detection and retained rather than silently removed.
- Exact duplicate rows were removed to avoid double-counting records. No other missing-value imputation was needed for this version of the data.

## Status

The notebook has been run with the downloaded CSV; analysis outputs and figures are saved inline in the notebook.
