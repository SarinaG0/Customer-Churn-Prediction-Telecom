# IBM Telco Customer Churn Dataset

## Dataset Overview

**Source:** IBM Telco Customer Churn (Kaggle / IBM Watson)  
**URL:** [Kaggle Dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)  
**Records:** 7,043 customers  
**Time Period:** Cross-sectional (snapshot data)  
**License:** Public Domain (IBM Sample Dataset)

---

## Dataset Structure

### Records
- **Total customers:** 7,043
- **Churned customers:** 1,869 (26.58%)
- **Retained customers:** 5,163 (73.42%)
- **Missing records removed:** 11 (new customers with 0 tenure)

### Features (21 attributes)

#### 1. Demographic Information (4 attributes)
| Attribute | Type | Values | Description |
|-----------|------|--------|-------------|
| `Gender` | Categorical | Male, Female | Customer gender |
| `SeniorCitizen` | Binary | 0, 1 | Whether customer is 65+ years old |
| `Partner` | Binary | Yes, No | Whether customer has a partner |
| `Dependents` | Binary | Yes, No | Whether customer has dependents |

#### 2. Service Information (9 attributes)
| Attribute | Type | Values | Description |
|-----------|------|--------|-------------|
| `PhoneService` | Binary | Yes, No | Whether customer has phone service |
| `MultipleLines` | Categorical | Yes, No, No phone service | Multiple phone lines |
| `InternetService` | Categorical | DSL, Fiber optic, No | Type of internet connection |
| `OnlineSecurity` | Categorical | Yes, No, No internet service | Online security add-on |
| `OnlineBackup` | Categorical | Yes, No, No internet service | Online backup add-on |
| `DeviceProtection` | Categorical | Yes, No, No internet service | Device protection plan |
| `TechSupport` | Categorical | Yes, No, No internet service | Technical support service |
| `StreamingTV` | Categorical | Yes, No, No internet service | TV streaming service |
| `StreamingMovies` | Categorical | Yes, No, No internet service | Movie streaming service |

#### 3. Account Information (3 attributes)
| Attribute | Type | Values | Description |
|-----------|------|--------|-------------|
| `Tenure` | Numeric | 1-72 months | Length of customer relationship |
| `Contract` | Categorical | Month-to-month, One year, Two year | Contract term |
| `PaperlessBilling` | Binary | Yes, No | Paperless billing enrollment |

#### 4. Billing Information (3 attributes)
| Attribute | Type | Values | Description |
|-----------|------|--------|-------------|
| `PaymentMethod` | Categorical | Electronic check, Mailed check, Bank transfer, Credit card | How customer pays |
| `MonthlyCharges` | Numeric | $18.25 - $118.75 | Average monthly bill |
| `TotalCharges` | Numeric | $18.80 - $8,684.80 | Total amount paid over tenure |

#### 5. Target Variable
| Attribute | Type | Values | Description |
|-----------|------|--------|-------------|
| `Churn` | Binary | Yes, No | Whether customer left within the last month |

---

## Data Characteristics

### Numerical Features Statistics
| Feature | Mean | Min | Max | Std Dev |
|---------|------|-----|-----|---------|
| Tenure (months) | 32.4 | 1 | 72 | 24.6 |
| Monthly Charges ($) | 64.79 | 18.25 | 118.75 | 30.09 |
| Total Charges ($) | 2,283.30 | 18.80 | 8,684.80 | 2,266.77 |

### Categorical Features Distribution
- **Senior Citizens:** 16.21% (1,142)
- **Gender:** 50.48% Male, 49.52% Female (balanced)
- **Partners:** 48.30% Yes, 51.70% No
- **Dependents:** 29.96% Yes, 70.04% No
- **Phone Service:** 90.32% Yes, 9.68% No
- **Internet Service:** 43.96% Fiber optic, 34.37% DSL, 21.67% No
- **Paperless Billing:** 59.22% Yes, 40.78% No
- **Payment Method:** 33.58% Electronic check (most common)

---

## Data Quality Notes

### Missing Values
- **TotalCharges:** 11 records with missing values
- **Reason:** New customers (tenure = 0) with no billing history yet
- **Action Taken:** Removed from modeling dataset

### Duplicates
- **No duplicate customer records** found

### Outliers
- **High monthly charges:** Some customers pay $100+ (legitimate premium service users)
- **Low tenure, high total charges:** Edge case but possible (high monthly charges × short tenure)

---

## Data Preprocessing Steps

1. **Removed 11 incomplete records** (tenure = 0, missing total charges)
2. **Excluded CustomerID** (non-informative identifier)
3. **Dummy coding** for categorical variables
4. **Set comparison groups** to avoid perfect collinearity:
   - "No internet service" as baseline for internet-dependent features
5. **Normalized numerical features** (tenure, charges) for regression models
6. **Applied SMOTE** to training set to balance 73/27 class distribution

---

## Key Correlations

**High Positive Correlations:**
- Fiber optic service - Monthly charges: **0.787**
- Total charges - Tenure: **0.826**
- Service add-ons - Monthly charges: **≥0.6**

**Perfect Collinearity (addressed via baseline coding):**
- "No internet service" values had perfect collinearity with "No" values within the same attribute

---

## How to Access the Data

1. **Download from Kaggle:**
   - Visit: https://www.kaggle.com/datasets/blastchar/telco-customer-churn
   - Download: `WA_Fn-UseC_-Telco-Customer-Churn.csv`
   - File size: ~1MB

2. **Alternative sources:**
   - IBM Watson Sample Datasets
   - RapidMiner Sample Repository

3. **For this project:**
   - Original CSV not included in repository due to size
   - All analysis performed in RapidMiner with data imported from Kaggle
