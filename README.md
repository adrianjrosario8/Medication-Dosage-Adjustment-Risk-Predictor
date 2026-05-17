# Medication Dosage Adjustment Risk Predictor

> A clinical ML system predicting dosage adjustment risk from patient-level EHR data - trained on 100K+ records, validated with three convergent methods, and deployed end-to-end on AWS via Docker.

[![Live App](https://img.shields.io/badge/Live%20App-Streamlit-FF4B4B?style=for-the-badge&logo=streamlit)](https://adrianjrosario8-medication-dosage-adjustment-risk-pr-app-rohyo4.streamlit.app/)
[![AWS](https://img.shields.io/badge/Deployed-AWS%20EC2-FF9900?style=for-the-badge&logo=amazonaws)](http://34.229.233.249:8501)
[![Docker](https://img.shields.io/badge/Docker-Hub-2496ED?style=for-the-badge&logo=docker)](https://hub.docker.com/r/adrianjrosario8/dosage-risk-app)
[![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python)](https://python.org)

---

## The Problem This Solves

Most EHR-based ML research on this dataset targets readmission prediction or glucose forecasting. This project reframes the problem around **dosage adjustment risk** - a clinically underserved prediction target directly tied to medication safety, adverse drug event prevention, and earlier clinical intervention.

Dosage adjustment decisions in hospital settings are complex, inconsistent, and difficult to scale. Clinicians simultaneously assess polypharmacy burden, comorbidities, glycemic control, and admission severity - often without structured decision support. This system provides that support.

---

## Model Performance

| Metric | Value |
|--------|-------|
| AUC-ROC | 0.812 (SD: ±0.0034) |
| High-Risk Recall (deployed, 8 features) | 0.87 |
| High-Risk Recall (full model, 21 features) | 0.906 |
| F1 Score | 0.75 |
| Accuracy | 0.722 |

**Why Recall is the headline metric:** In clinical ML, missing a high-risk patient is far more costly than a false positive. The model is explicitly optimised for recall on high-risk cases - the appropriate trade-off for a safety-critical screening tool.

**Validation approach:** AUC 0.812 confirmed across three independent methods - Nested CV, Stratified K-Fold, and Repeated Stratified K-Fold - with near-identical standard deviations (AUC SD: 0.0035). Convergence across three methods is strong evidence of a stable, non-overfit model.

**Feature reduction:** The deployed model uses 8 clinically interpretable features selected from a 21-feature full model, retaining 97% of full-model recall while maximising transparency and clinical usability.

---

## Example Output

```
Risk Classification:    HIGH RISK
Probability (High):     98.47%
Probability (Low):       1.53%

Clinical Decision Support:
- Patient is on diabetes medication, increasing sensitivity to dosage changes
- High medication burden (20+ medications) indicates elevated treatment complexity
- Elevated A1C indicates suboptimal glycemic control
- Extended hospital stay may indicate higher condition severity
```

---

## Clinical Features

| Feature | Clinical Rationale |
|---------|-------------------|
| Diabetes medication usage | Increases sensitivity to dosage changes |
| Total medications (polypharmacy) | Indicator of treatment complexity |
| Medication burden (20+ meds) | Elevated interaction and adjustment risk |
| A1C levels | Glycemic control and dosage stability |
| Prior procedures during admission | Proxy for clinical severity |
| Length of hospital stay | Condition severity indicator |
| Age | Physiological risk modifier |
| Number of diagnoses | Comorbidity burden |

---

## Deployment Architecture

```
User → AWS EC2 → Docker Container → Streamlit → XGBoost Model
```

This project is deployed end-to-end across three environments:

| Environment | Link |
|-------------|------|
| Streamlit Cloud | [Live App](https://adrianjrosario8-medication-dosage-adjustment-risk-pr-app-rohyo4.streamlit.app/) |
| AWS EC2 | [http://34.229.233.249:8501](http://34.229.233.249:8501) |
| Docker Hub | [adrianjrosario8/dosage-risk-app](https://hub.docker.com/r/adrianjrosario8/dosage-risk-app) |

---

## Run With Docker

Pull and run the public image in one command - no setup required:

```bash
docker pull adrianjrosario8/dosage-risk-app
docker run -p 8501:8501 adrianjrosario8/dosage-risk-app
```

Open `http://localhost:8501` in your browser.

This is reproducible deployment in practice - the same image runs identically on any system, matching what is live on AWS.

---

## Run Locally

```bash
git clone https://github.com/adrianjrosario8/Medication-Dosage-Adjustment-Risk-Predictor.git
cd Medication-Dosage-Adjustment-Risk-Predictor
pip install -r requirements.txt
streamlit run app.py
```

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Model | XGBoost, scikit-learn |
| Data Processing | pandas, NumPy |
| Validation | Nested CV, Stratified K-Fold, Repeated Stratified K-Fold, Brier Score |
| Frontend | Streamlit |
| Containerization | Docker, Docker Hub |
| Cloud Deployment | AWS EC2 |

---

## What Differentiates This Project

- **Novel target:** Dosage adjustment risk as a prediction target is underexplored - prior work on this dataset focused almost exclusively on readmission and glucose prediction. Target variable selection was driven by domain research into clinically underserved gaps.
- **Rigorous validation:** Three-method convergence at AUC 0.812 with near-identical standard deviations rules out overfitting and confirms generalisability.
- **Clinical reasoning layer:** Predictions are accompanied by structured clinical explanations grounded in actual hospital risk assessment logic - not black-box scores.
- **Full MLOps pipeline:** Local development, Docker containerization, Docker Hub registry, and AWS EC2 deployment - a complete production deployment workflow.
- **Recall-optimised design:** Model architecture reflects clinical priorities, not just benchmark metrics.

---

## Future Improvements

- FastAPI inference endpoint for REST API serving
- MLflow experiment tracking and model registry for versioned model management
- CI/CD deployment pipeline
- Model monitoring and drift detection
- Batch hospital workflow integration

---

## Clinical Note

This tool is intended for **clinical decision support only** and does not replace professional medical judgment. All predictions should be interpreted in the context of full patient assessment by a qualified clinician.

---

## Author

**Adrian Jacob Rosario**
MS Pharmaceutical Sciences - Pharmacometrics & Systems Pharmacology, University of Pittsburgh

Building end-to-end pharmacovigilance ML systems at the intersection of pharmaceutical research and production ML engineering.

[GitHub Portfolio](https://github.com/adrianjrosario8) | [LinkedIn](https://www.linkedin.com/in/adrian-jacob-rosario-330a47235/) | [Docker Hub](https://hub.docker.com/r/adrianjrosario8/dosage-risk-app)
