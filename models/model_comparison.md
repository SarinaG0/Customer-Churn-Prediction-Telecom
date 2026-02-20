# Model Performance Comparison

## Summary Table

| Model | Accuracy | AUC | Precision | Recall | F1-Score | Specificity | Classification Error |
|-------|----------|-----|-----------|--------|----------|-------------|----------------------|
| **Logistic Regression*** | **76.97%** | **86.1%** | **54.50%** | **81.02%** | **65.16%** | **75.51%** | **23.03%** |
| Linear Regression | 75.55% | 84.1% | 52.66% | 79.41% | 63.33% | 74.15% | 24.45% |
| Random Forest | 73.49% | 83.9% | 50.08% | 84.22% | 62.81% | 69.60% | 26.51% |
| Decision Tree | 74.20% | 80.1% | 50.92% | 81.02% | 62.54% | 71.73% | 25.80% |

---

## Model Details

### 1. Logistic Regression (RECOMMENDED)*

**Configuration:**
- Threshold: 0.5
- First class: "No" (not churned)
- Second class: "Yes" (churned)
- SMOTE upsampling applied to training data

**Performance:**
- **Accuracy:** 76.97%
- **AUC:** 86.1% (highest)
- **Precision:** 54.50% (highest)
- **Recall:** 81.02%
- **F1-Score:** 65.16% (highest)
- **Specificity:** 75.51% (highest)

**Strengths:**
- Best overall discriminatory ability (highest AUC)
- Highest precision (minimizes false positives/wasted retention efforts)
- Strong recall (captures 81% of actual churners)
- High interpretability (clear coefficient interpretation)
- Best balance between precision and recall

**Use Case:** Production deployment for churn prediction system

---

### 2. Linear Regression

**Configuration:**
- SMOTE upsampling applied
- Continuous output converted to binary via threshold

**Performance:**
- **Accuracy:** 75.55%
- **AUC:** 84.1%
- **Precision:** 52.66%
- **Recall:** 79.41%
- **F1-Score:** 63.33%
- **Specificity:** 74.15%

**Strengths:**
- Good coefficient interpretability
- Provides continuous churn probability scores
- Strong statistical foundation

**Weaknesses:**
- Lower than Logistic Regression across all metrics
- Not designed for binary classification (logistic preferred)

**Use Case:** Exploratory analysis, understanding linear relationships

---

### 3. Random Forest

**Configuration:**
- Ensemble of decision trees
- SMOTE upsampling applied

**Performance:**
- **Accuracy:** 73.49% (lowest)
- **AUC:** 83.9%
- **Precision:** 50.08% (lowest)
- **Recall:** 84.22% (highest)
- **F1-Score:** 62.81%
- **Specificity:** 69.60% (lowest)

**Strengths:**
- Highest recall (captures 84.22% of churners)
- Robust to overfitting (in theory)
- Handles non-linear relationships well

**Weaknesses:**
- Lowest precision (many false positives)
- Lowest specificity (poor at identifying non-churners)
- Lower accuracy suggests possible overfitting
- "Black box" model (harder to interpret)

**Use Case:** When recall is paramount (e.g., high-value customer retention)

---

### 4. Decision Tree

**Configuration:**
- Criterion: Gain ratio
- Max depth: 15
- Pruning: Enabled
- Pre-pruning confidence: 0.25

**Performance:**
- **Accuracy:** 74.20%
- **AUC:** 80.1% (lowest)
- **Precision:** 50.92%
- **Recall:** 81.02%
- **F1-Score:** 62.54%
- **Specificity:** 71.73%

**Strengths:**
- Highly interpretable (visual decision tree)
- Easy to explain to non-technical stakeholders
- Fast prediction time

**Weaknesses:**
- Lowest AUC (poorest discriminatory ability)
- Prone to overfitting without pruning
- Lower performance than ensemble methods

**Use Case:** Exploratory analysis, rule extraction, stakeholder demos

---

## Why Logistic Regression Was Selected

### Decision Criteria

1. **Highest AUC (86.1%):** Best ability to rank customers by churn probability
2. **Lowest classification error (23.03%):** Fewest overall mistakes
3. **Best F1-Score (65.16%):** Optimal precision-recall balance
4. **High interpretability:** Stakeholders can understand which features drive churn
5. **Business applicability:** 81% recall captures most churners while 54.5% precision minimizes wasted retention spend

### Business Trade-offs

**Why Not Random Forest (despite higher recall)?**
- Lower precision (50.08% vs. 54.50%) means more false alarms
- Lower specificity (69.60% vs. 75.51%) means poorer identification of loyal customers
- Harder to explain predictions to business stakeholders
- Lower overall accuracy suggests model instability

**The Business Impact:**
- **Random Forest:** Contacts 1,000 predicted churners → 501 are false positives (wasted effort)
- **Logistic Regression:** Contacts 1,000 predicted churners → 455 are false positives (better efficiency)

---

## Evaluation Metrics Explained

| Metric | Formula | Business Meaning |
|--------|---------|------------------|
| **Accuracy** | (TP + TN) / Total | Overall correct predictions |
| **Precision** | TP / (TP + FP) | When model predicts churn, how often is it right? |
| **Recall** | TP / (TP + FN) | Of all actual churners, how many did we catch? |
| **F1-Score** | 2 × (Precision × Recall) / (Precision + Recall) | Balanced precision-recall metric |
| **Specificity** | TN / (TN + FP) | Of all loyal customers, how many did we correctly identify? |
| **AUC** | Area under ROC curve | Ability to distinguish churners from non-churners |

**Legend:** TP = True Positive, TN = True Negative, FP = False Positive, FN = False Negative

---

## Confusion Matrix Example (Logistic Regression)

|  | **Predicted: No Churn** | **Predicted: Churn** |
|--|-------------------------|----------------------|
| **Actual: No Churn** | 1,033 (TN) | 335 (FP) |
| **Actual: Churn** | 85 (FN) | 361 (TP) |

**Interpretation:**
- **True Negatives (1,033):** Correctly identified loyal customers
- **True Positives (361):** Correctly caught 81% of churners
- **False Positives (335):** Predicted churn but customer stayed (retention effort wasted)
- **False Negatives (85):** Missed 19% of churners (lost revenue opportunity)

---

## Recommendations

**For Production Deployment:**
- Use **Logistic Regression** for primary churn scoring
- Set threshold at 0.5 or optimize based on business cost-benefit analysis

**For High-Value Customers:**
- Consider **Random Forest** to maximize recall (capture more churners)
- Accept lower precision trade-off for customers with high CLV

**For Explainability:**
- Use **Logistic Regression** coefficients to explain predictions to executives
- Create "churn risk scorecards" based on coefficient weights

**For Real-Time Scoring:**
- Deploy Logistic Regression via API (fast prediction, low latency)
- Integrate with CRM for automated alerts when churn probability >0.7
