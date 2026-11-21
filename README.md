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
- Excluded name, doctor, hospital, and room number from the analytical frame. The cleaned analytical table contains 54,966 rows and 12 columns.
- Length of stay is spread fairly evenly across 1–30 days. Admissions are also distributed fairly evenly across the six medical conditions and three admission types.
- Age, billing amount, and length of stay have very weak linear relationships with one another. The largest absolute pairwise correlation is about 0.008.
- Median billing ranges from about $24,911 for Cancer to $26,146 for Obesity. The roughly $1,235 gap is small relative to the overall billing interquartile range of about $24,576.
- Test results are nearly balanced overall at 33.54% Abnormal, 33.35% Normal, and 33.11% Inconclusive. The largest difference between a subgroup rate and its overall rate is 1.01 percentage points, for Abnormal results among hypertension admissions (32.53% compared with 33.54% overall). The largest differences by age group and admission type are 0.87 and 0.34 percentage points, respectively.
- Six figures and the test-result comparison table are saved inline. The full results are in the notebook’s Findings and Interpretation section. [Deliverable_1_EDA.ipynb](Deliverable_1_EDA.ipynb)

## Modeling next steps

The proposed prediction task uses age and admission type to estimate whether the recorded test result will be Abnormal, Normal, or Inconclusive. Medical condition is eligible only if it was recorded by admission. Exclude discharge date, length of stay, and billing amount because their recorded values may reflect information from later in the stay.

The small subgroup differences make a majority-class baseline a useful benchmark. Fit that baseline and a multinomial logistic regression on earlier admissions, then compare their accuracy and macro F1 on later admissions. The 33.54% overall share for Abnormal is a descriptive reference. Calculate the baseline's held-out accuracy using the majority class from the training data.

## Challenges and decisions

- The dataset is synthetic, so findings cannot be generalized to real patients or healthcare systems.
- Negative billing values have no explanatory metadata. They are reported separately from statistical outlier detection and retained rather than silently removed.
- Exact duplicate rows were removed to avoid double-counting records. No other missing-value imputation was needed for this version of the data.

## Status

The notebook has been run with the downloaded CSV; analysis outputs and figures are saved inline in the notebook.
