# Predicting Hospital Readmission of Diabetic Patients

**Author:** Suryansh Mishra · [GitHub](https://github.com/Suryansh2111) · [LinkedIn](https://www.linkedin.com/in/suryansh-mishra-bb3879339/)

## Problem

Hospitals are penalized under Medicare's Hospital Readmissions Reduction Program when too many patients return within 30 days of discharge. Diabetic patients are especially at risk. This project builds a model that ranks diabetic patients by their risk of 30-day readmission, so care teams can follow up with the highest-risk patients before and after discharge.

## Data

[Diabetes 130-US Hospitals (1999-2008)](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008), UCI Machine Learning Repository: 101,766 encounters, 50 features.

Target: `readmitted` = 1 if readmitted within 30 days, 0 otherwise (no readmission, or after 30 days).

## Approach

**Cleaning**
- Dropped columns that are mostly missing (`weight`, `payer_code`, `medical_specialty`) or constant (`citoglipton`, `examide`).
- Removed rows with missing race or diagnosis codes and unknown gender.
- Removed patients who died (discharge code 11), since they cannot be readmitted.
- Kept one encounter per patient (67,580 patients) so no patient appears in both training and test data.

**Feature engineering**
- `patient_visit` = prior outpatient + emergency + inpatient visits.
- Grouped hundreds of ICD-9 diagnosis codes into 9 clinical categories.
- Merged similar admission and discharge codes; encoded medication changes (No / Steady / Down / Up).
- Removed collinear features using variance inflation factor (VIF).

**Modeling**
- Stratified 80/20 train/test split; the test set keeps the real 9.1% readmission rate.
- Identifier columns removed before modeling.
- Imbalance handled with class weights; compared against SMOTE applied to the training split only.
- Models: Decision Tree, Logistic Regression, Random Forest.
- Evaluated with ROC-AUC, PR-AUC, recall and precision for the readmitted class, and lift at capacity-based thresholds.

## Results (test set, real 9.1% readmission rate)

| Model | ROC-AUC | PR-AUC | Recall (readmit) | Precision (readmit) |
| --- | --- | --- | --- | --- |
| Random Forest | **0.630** | **0.150** | 0.396 | 0.146 |
| Decision Tree | 0.620 | 0.135 | 0.488 | 0.134 |
| Logistic Regression | 0.592 | 0.122 | 0.537 | 0.116 |

A random model's PR-AUC is 0.091.

**Using it in practice:** flagging the top 20% highest-risk patients catches **34%** of 30-day readmissions at **1.7x** the base rate. The top 10% gives about **2x** lift.

**Top drivers** (permutation importance): discharge disposition, prior hospital visits, number of diagnoses, age.

**SMOTE vs class weights:** SMOTE lowered ROC-AUC from 0.63 to 0.51. Interpolating label-encoded categories (diagnosis group, discharge code) creates values that never occur in real data, so the model learns to spot synthetic patients. Class weights avoid this.

## Business impact

The model gives care coordinators a ranked list of patients at discharge. With capacity to follow up on 20% of discharges, they would reach about a third of the patients who would otherwise be readmitted, through follow-up calls, medication review and early clinic visits.

## Next steps

- One-hot encode categorical features and add gradient boosting (XGBoost / LightGBM) with tuning.
- Use all encounters with group-aware cross-validation by patient.
- Add SHAP explanations for patient-level reasons.

## Note on earlier versions

An earlier version of this notebook reported about 90% accuracy. That came from applying SMOTE before the train/test split and testing on balanced data. The current notebook fixes this, and the results above are the valid ones.

## Files

- `Hospital_Readmission_of_Diabetic_Patient.ipynb`: full analysis
- `diabetic_data.csv`, `IDs_mapping.csv`: data from UCI

**Tools:** Python, pandas, scikit-learn, imbalanced-learn, statsmodels, seaborn
