# Medication Dosage Adjustment Risk Predictor

> A clinical decision support system that predicts high vs. low dosage adjustment risk from patient-level clinical data, combining XGBoost-based risk classification with human-readable clinical explanations.

**[Live Demo](https://adrianjrosario8-medication-dosage-adjustment-risk-pr-app-rohyo4.streamlit.app/)** - Deployed and fully functional on Streamlit Cloud

---

## Problem Statement

Medication dosage adjustment decisions in hospital settings are complex, inconsistent, and difficult to scale. Clinicians must simultaneously assess polypharmacy burden, comorbidities, glycemic control, and admission severity, often without structured decision support. This creates risk of under-treatment, over-treatment, and delayed intervention in high-risk patients.

---

## Solution

A machine learning pipeline trained on real-world clinical EHR data that classifies patients as high or low dosage adjustment risk and generates structured clinical reasoning for each prediction.

```
Patient Input → Feature Engineering → XGBoost Model → Risk Classification → Clinical Explanation
```

The system prioritizes **high recall on high-risk patients**, the clinically critical direction  ensuring that genuinely at-risk patients are flagged rather than missed.

---

## Example Output

```
Risk Classification:    HIGH RISK 🚨
Probability (High):     98.47%
Probability (Low):       1.53%

Clinical Decision Support:
- Patient is on diabetes medication, increasing sensitivity to dosage changes
- High medication burden (20+ medications) indicates increased treatment complexity
- Elevated A1C indicates suboptimal glycemic control
- Extended hospital stay may indicate higher severity of condition
```

---

## Model Performance

| Metric | Value |
|--------|-------|
| AUC-ROC | 0.812 (+/- 0.0034) |
| Accuracy | 0.722 |
| F1 Score | 0.75 |
| High Risk Recall (full model, 21 features) | 0.906 |
| High Risk Recall (deployed model, 8 features) | 0.87 |
| High Risk Precision | 0.641 |
| Low Risk Recall | 0.59 |
| Low Risk Precision | 0.84 |

The model is optimized for **high recall on high-risk cases** (0.87), accepting lower precision to minimize missed detections which is the appropriate clinical trade-off for a safety-critical screening tool. Stable validation confirmed across cross-validation folds (AUC SD: 0.0035).

---

## Clinical Features Used

| Feature | Clinical Relevance |
|---------|-------------------|
| Diabetes medication usage | Increases sensitivity to dosage changes |
| Total medications (polypharmacy) | Indicator of treatment complexity |
| Medication burden (20+ meds) | Elevated interaction and adjustment risk |
| A1C levels | Glycemic control and dosage stability |
| Prior history of procedures during admission | Proxy for clinical severity |
| Length of hospital stay | Indicator of condition severity |
| Age | Physiological risk modifier |
| Number of diagnoses | Comorbidity burden |

---

## Why This Project Stands Out

Most EHR-based ML projects focus on readmission prediction or glucose forecasting. This system targets a clinically underserved problem, **dosage adjustment risk** which directly affects medication safety decisions. Key differentiators:

- Trained on a dataset of 100,000+ patient records with rigorous cross-validation
- Clinically grounded feature engineering reflecting actual hospital risk assessment logic
- Explainable outputs designed for decision support, not black-box scoring
- Recall-optimized model design aligned with clinical safety priorities
- Stratified K-Fold, Repeated Stratified K-Fold, and Nested CV all converging around 0.812 AUC with near-identical standard deviations is strong evidence of a stable, non-overfit model.
- The deployed model uses 8 clinically interpretable features selected from a 21-feature full model, retaining 97% of recall performance while maximising transparency and clinical usability.
- Target variable selection was driven by domain research, dosage adjustment risk was identified as a clinically underserved problem in this dataset, previously used almost exclusively for readmission prediction.

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Model | XGBoost, Scikit-learn |
| Data Processing | Pandas |
| Frontend & Deployment | Python, Streamlit, Streamlit Cloud |
| Validation | Nested Cross-validation, Brier Score, AUC-ROC |

---

## How to Run Locally

```bash
git clone https://github.com/adrianjrosario8/Medication-Dosage-Adjustment-Risk-Predictor.git
cd Medication-Dosage-Adjustment-Risk-Predictor
pip install -r requirements.txt
streamlit run app.py
```

---

## Future Improvements

- SHAP-based feature importance visualization
- Confidence intervals on risk probability outputs
- Multi-condition and multi-drug risk modeling
- REST API deployment for EHR system integration
- Standardized clinical documentation export

---

## Clinical Note

This tool is intended for **clinical decision support only** and does not replace professional medical judgment. All predictions should be interpreted in the context of full patient assessment by a qualified clinician.

---

## Author

Built as part of a portfolio focused on **Healthcare AI**, **Clinical Data Science**, and **Pharmacovigilance ML**.

[GitHub Portfolio](https://github.com/adrianjrosario8) | [LinkedIn](www.linkedin.com/in/adrian-jacob-rosario-330a47235)
