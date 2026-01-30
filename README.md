# ai-loan-approval-databricks
End-to-end AI-driven loan approval and risk decisioning system built on Databricks using the Medallion architecture, Delta Lake, MLflow, and policy impact analysis.

# 🚀 Explainable AI Loan Approval System (Databricks + Delta Lake)

## 📌 Problem Definition & AI Framing

Traditional loan approval systems rely heavily on rigid rules or opaque models, which leads to:

- ❌ Lack of transparency in decisions  
- ❌ Inconsistent approval policies  
- ❌ Difficulty in measuring business impact of rule changes  

### 🎯 Objective

Build an **explainable AI-driven loan approval system** that:

- Predicts loan approval probability using machine learning  
- Converts probabilities into business-friendly decisions  
- Allows policy simulation (strict vs lenient)  
- Is production-ready using **Databricks + Delta Lake**

---

## 📊 Dataset Source

The dataset used in this project was sourced from **Kaggle**.

- **Dataset Name:** Loan Prediction Problem Dataset 
- **Dataset Link:**  
  https://www.kaggle.com/datasets/altruistdelhite04/loan-prediction-problem-dataset 

The dataset contains structured loan application records such as applicant income, credit history, employment details, and loan attributes.  
It is suitable for demonstrating **credit risk modeling, explainable ML, and policy-driven decision systems** in a real-world context.


## 🧱 Data Architecture (Medallion Design)

This project follows a **Bronze → Silver → Gold** architecture.

Raw CSV
  ↓
Bronze (Raw Delta Table)
  ↓
Silver (Cleaned + Feature Engineered)
  ↓
Gold (ML Decisions + Policy Simulation + Analytics)


### Why Medallion Architecture?

- Clear separation of concerns  
- Auditable transformations  
- Scalable and production-ready design  

---

## 🥉 Bronze Layer – Raw Data Ingestion

### Purpose
Store raw loan application data with **minimal transformation**.

### Key Actions
- Read raw CSV from Databricks Volume  
- Infer schema automatically  
- Store data as a Delta table  

### Table Created
- **`bronze_loan_applications`**

### Benefits
- ✔ Preserves raw data  
- ✔ Acts as immutable source of truth  

---

## 🥈 Silver Layer – Feature Engineering & Cleaning

### Purpose
Prepare **high-quality, explainable features** for ML training and analytics.

### Transformations Performed
- Dropped rows with missing critical fields  
- Engineered domain-driven features:
  - `total_income`
  - `loan_to_income_ratio`
  - `credit_flag`
  - `employment_flag`
- Retained numeric & business-interpretable features only  

### Table Created
- **`silver_loan_features`**

### Benefits
- ✔ Clean  
- ✔ ML-ready  
- ✔ Business-interpretable  

---

## 🥇 Gold Layer – ML Training, Risk Scoring & Decisions

The Gold layer is implemented in **three phases**.

---

### 🔹 Phase 1: Model Training & Evaluation

#### Model Used
**Logistic Regression** (Explainable & Interpretable)

#### Why Logistic Regression?
- Produces probabilities (not just labels)  
- Coefficients explain feature impact  
- Well-accepted in regulated domains (finance)  

#### Steps
- Created binary label: `loan_approved_flag`  
- Feature assembly using `VectorAssembler`  
- Explicit null handling  
- Train/Test split (80/20)  
- Model evaluation using AUC  

#### Metric Used
| Metric | Description |
|------|-------------|
| AUC | Area Under ROC Curve |

✔ Transparent  
✔ Explainable  
✔ Suitable for business decisions  

---

### 🔹 Phase 2: Explainability & Risk Scoring

#### Explainability
- Extracted model coefficients  
- Mapped coefficients to feature importance  

#### Risk Band Mapping

| Approval Probability | Risk Band |
|---------------------|-----------|
| ≥ 0.75 | LOW_RISK |
| 0.50 – 0.75 | MEDIUM_RISK |
| < 0.50 | HIGH_RISK |

#### Business Decisions

| Risk Band | Decision |
|---------|----------|
| LOW_RISK | AUTO_APPROVE |
| MEDIUM_RISK | MANUAL_REVIEW |
| HIGH_RISK | REJECT |

---

### 🔹 Phase 3: Production Scoring & Policy Simulation

#### Production Scoring
- Applied trained model to full Silver dataset  
- Generated final decisions for all applicants  

#### Gold Table Created
- **`gold_loan_decisions`**

#### Columns
- `loan_id`  
- `approval_probability`  
- `risk_band`  
- `decision`  

---

## 📊 Policy Simulation (Business Impact)

Two alternative business policies were simulated.

### 🔒 Strict Policy
- AUTO_APPROVE ≥ 0.85  
- MANUAL_REVIEW ≥ 0.60  

### 🎯 Lenient Policy
- AUTO_APPROVE ≥ 0.65  
- MANUAL_REVIEW ≥ 0.45  

### Insights Generated
- Approval count increase/decrease  
- Risk vs growth trade-offs  
- Impact of policy tightening or relaxation  

✔ Directly useful for business stakeholders  

---

## 📈 Analytics & Visualizations (Databricks Native)

All analytics were built using **Databricks SQL + `display()` visuals**.

### Examples
- Auto-approval rate (% of applicants automatically approved)  
- Decision distribution across risk bands (LOW_RISK, MEDIUM_RISK, HIGH_RISK)  
- Number of decisions impacted under stricter policy rules  
- Breakdown of decision changes (baseline policy vs strict policy)
  
---

## 🗄️ Delta Lake Features Used

- ACID transactions  
- Schema enforcement  
- Schema evolution (`overwriteSchema`)  
- Optimized reads for analytics  

---

## 🔁 Orchestration (Databricks Jobs)

- Created a Databricks Job  
- Executed full pipeline end-to-end  
- Manual run completed successfully  

**Execution Time:** ~1 min 48 sec  

✔ Production-ready workflow  

---

## 🔐 Governance & Catalog

- Tables stored under **Unity Catalog**  
- Clear separation by layer  
- Ready for permission-based access control  

---

## 🤖 MLflow Integration

- Model and metrics logged using **MLflow**  
- Experiment tracking enabled  
- Model reproducibility ensured  
- UC volume path configured for Spark ML logging  

---

## 📌 Business Impact & Practical Use

This system enables:

- Faster loan decisions  
- Transparent approvals  
- Risk-aware policy design  
- Easy simulation of rule changes  
- Scalable ML-driven workflow  

### Real-World Use Cases
- Retail Banking  
- NBFCs  
- Fintech Loan Platforms  
- Credit Risk Teams  

---
## 🏁 Final Note

Although the dataset is small, this project demonstrates:

- Enterprise-grade architecture  
- Correct ML design choices  
- Strong business alignment  
- Production-ready Databricks workflow  



