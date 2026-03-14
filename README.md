# Predicting-Hospital-Readmission-of-Diabetic-Patient
Problem

Hospital readmissions within 30 days of discharge are a major concern for healthcare systems. High readmission rates often indicate gaps in patient care and lead to increased healthcare costs and financial penalties for hospitals.

Diabetic patients are particularly vulnerable to complications that may result in repeated hospital visits. Being able to predict which patients are likely to be readmitted allows hospitals to intervene early through follow-ups, medication adjustments, or additional care.

The goal of this project is to build a machine learning model that predicts the likelihood of a diabetic patient being readmitted within 30 days after discharge.

Dataset

The project uses the Diabetes 130-US Hospitals dataset from the UCI Machine Learning Repository.

Dataset highlights:

101,766 patient records

50 clinical and demographic features

Data collected from 130 US hospitals (1999–2008)

Features include patient demographics, admission details, lab results, number of procedures, diagnoses, and medication changes.

The target variable was converted into a binary classification problem:

1 → Readmitted within 30 days

0 → No readmission or readmission after 30 days

Approach

An end-to-end machine learning pipeline was developed.

Data Preparation

Removed columns with high missing values

Cleaned corrupted entries and duplicates

Removed deceased patients (readmission not possible)

Feature Engineering

Created a patient_visit feature combining inpatient, outpatient, and emergency visits

Grouped hundreds of ICD-9 diagnosis codes into 9 major disease categories

Encoded categorical variables such as gender, age, and medication changes

Handling Class Imbalance

Since the dataset contained more non-readmitted patients, SMOTE was used to balance the classes.

Models Implemented

Three models were trained and evaluated:

Decision Tree

Logistic Regression

Random Forest

Model Performance
Model	Accuracy	Precision	Recall	F1 Score
Decision Tree	78%	0.77	0.76	0.76
Logistic Regression	73%	0.72	0.71	0.71
Random Forest	84%	0.83	0.82	0.82

The Random Forest model achieved the best performance, effectively identifying patients at risk of early readmission.

Key Insights

Exploratory analysis showed that readmission risk increases for patients with:

Multiple diagnoses

Longer hospital stays

Frequent prior hospital visits

Certain medication adjustments

Business Impact

This predictive model can help hospitals:

Identify high-risk patients before discharge

Improve post-care monitoring and follow-ups

Reduce costly hospital readmissions

Support data-driven healthcare decision making

By proactively identifying at-risk patients, healthcare providers can improve patient outcomes while reducing operational costs.
