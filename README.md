# MSCS 634 — Project Deliverable 1

## Dataset overview and selection

This deliverable uses the Wisconsin Diagnostic Breast Cancer (WDBC) dataset. It contains 569 observations and 30 numeric predictors, plus a binary diagnosis label (`target`), meeting the project requirement of at least 500 records and 8–10 attributes.

The measurements were computed from digitized images of fine-needle aspirate (FNA) samples of breast masses. Ten characteristics of cell nuclei are represented: radius, texture, perimeter, area, smoothness, compactness, concavity, concave points, symmetry, and fractal dimension. Each characteristic appears as a mean, standard error, and “worst” value, producing 30 measurements. In this dataset, “worst” refers to the mean of the three largest values for that characteristic, not necessarily a single maximum.

The dataset is appropriate for studying binary classification because it has a documented diagnosis target and numerical predictors. Its measurements also support exploratory clustering and feature-relationship analysis. The data are less naturally suited to regression or association rules: later work should define a defensible continuous quantity to predict and discretize features transparently before mining rules, rather than implying that a numeric measurement is a clinical outcome.

**Target encoding:** `0` = malignant and `1` = benign. There are 212 malignant cases and 357 benign cases. This is a moderately uneven class distribution, so later classification should use stratified splits and report class-sensitive metrics (such as recall, precision, and F1), not accuracy alone.

**Source and scope:** The included CSV is based on the UCI Machine Learning Repository’s [Breast Cancer Wisconsin (Diagnostic) dataset](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic), also available through scikit-learn’s `load_breast_cancer` dataset interface. The dataset is for coursework and should not be used for diagnosis, treatment, or other clinical decisions.

## Loading and cleaning

The notebook loads `data/breast_cancer.csv` with Pandas and checks its dimensions, column types, summary statistics, missing values, duplicate rows, and the set of diagnosis labels.

| Check or action | Result and rationale |
|---|---|
| Dataset dimensions | 569 rows × 31 columns (30 predictors and target). |
| Missing values | None found. A median-imputation fallback is included for numeric fields if missing values are encountered in a changed input file; it is not applied to this dataset. |
| Exact duplicate rows | None found. `drop_duplicates()` is retained so repeated records are removed if present in a future version of the input. |
| Target consistency | An assertion checks that the target contains only the expected binary values, `0` and `1`. |
| Potential outliers | The 1.5×IQR rule is used to flag observations for review. Flagged values are not automatically deleted: unusual measurements can be valid samples, and the dataset does not provide enough context to label them as errors. |
| Cleaned output | The resulting table is written to `data/cleaned_breast_cancer.csv`. |

No values required correction or imputation in this version. Outlier flags are diagnostic indicators, not proof of noisy or invalid data; extreme observations should be checked against domain knowledge and their effect tested during modeling.

## EDA findings and interpretation

The notebook includes a diagnosis-count bar chart, a mean-radius distribution by diagnosis, a mean-concavity boxplot, and a correlation heatmap for selected measurements.

* **Diagnosis counts:** The 357 benign and 212 malignant observations indicate that benign cases are more common (about 62.7%). This is not an extreme imbalance, but a classifier that predicts the majority class can appear more successful than its malignant-case detection warrants. Use stratified evaluation and inspect malignant recall and the confusion matrix.
* **Radius by diagnosis:** Comparing the distributions shows how mean radius varies across the two labels. This can motivate a baseline classifier, but overlap between the distributions means radius alone should not be treated as a definitive decision rule.
* **Concavity and unusual values:** The boxplot makes spread and potential extremes visible. IQR-flagged values should be reviewed rather than removed mechanically; compare model results with and without any justified treatment of flagged observations.
* **Feature relationships:** Mean radius and mean perimeter have a correlation of 0.9979. Mean radius, perimeter, and area capture closely related size information, so including all of them may create multicollinearity in regression and redundant weighting in distance-based methods. Later modeling can compare a regularized model or selected feature set against a full-feature baseline. Correlation is association, not evidence that one feature causes another or that any feature is clinically useful.

These are exploratory patterns in a small, historical dataset. The notebook’s plots help identify modeling questions; they do not establish clinical performance or generalizability.

## Challenges and limitations

1. **Correlated predictors:** Several measurements describe related aspects of cell size and shape. This complicates coefficient interpretation and can affect regression and clustering. Compare full and reduced/regularized feature sets, and avoid interpreting correlated coefficients independently.
2. **Different measurement scales:** Features range from small decimal-valued measurements to areas in the thousands. Standardization should be fit on training data within a pipeline before applying scale-sensitive models such as logistic regression with regularization, K-means, or PCA.
3. **Moderate class imbalance:** Benign observations outnumber malignant observations. Use stratified train/test splits and class-aware metrics; consider the consequences of false negatives rather than optimizing accuracy alone.
4. **Outliers versus valid variation:** IQR can flag legitimate observations, particularly in skewed measurements. Keep the observations unless a data error is demonstrated, and report sensitivity analysis if a later model applies an outlier treatment.
5. **Limited sample size and scope:** With 569 observations, model estimates may vary across splits. The dataset also lacks demographic variables needed to examine subgroup fairness and does not represent every population or modern clinical setting.
6. **Task fit and interpretation:** The diagnosis label supports classification, but regression on a feature would predict a measurement rather than a clinical outcome. Association rules require deliberate discretization of continuous measurements and careful exclusion or separate treatment of the diagnosis label to avoid misleading rules.
7. **Provenance and clinical use:** The dataset is used here as a learning resource. Its historical source and limited context mean results should not be presented as current medical guidance or evidence of real-world diagnostic effectiveness.

## Reproducibility

Install the listed packages with `pip install -r requirements.txt`, then open `analysis.ipynb` and run all cells from this directory. The source and cleaned CSVs are stored in the `data/` subdirectory.
