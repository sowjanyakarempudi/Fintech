# NextGen Wealth: AI-Driven Customer Churn Reduction
## ML Project Plan & Presentation

---

## Executive Summary
**Company:** NextGen Wealth - FinTech robo-advisory platform  
**Goal:** Reduce customer churn by 20% in the next fiscal year using AI  
**Timeline:** Multi-phase ML project implementation

---

## STAGE 1: BUSINESS & DATA UNDERSTANDING

### 1. Define the Precise ML Problem
**Learning Paradigm:** Supervised Learning  
**Task Type:** Binary Classification
- **Target Variable:** Customer churn (Yes/No, 1/0)
- **Prediction Window:** 30/60/90 days ahead (to be determined)
- **Problem Formulation:** Identify high-risk customers before they close accounts

**Rationale:**
- Historical churn data available
- Clear binary outcome
- Enables proactive intervention

---

### 2. Define the Target Label
**Primary Label:** `churned` (Boolean)
- **Definition:** Customer account closed by customer-initiated request
- **Data Source:** Historical account closure records
- **Temporal Boundary:** Look-back period of 12-24 months
- **Exclusion Criteria:** 
  - Accounts closed due to fraud
  - Dormant accounts (no activity for 6+ months)
  - Accounts with pending disputes

---

### 3. Business Use Case & Action Framework

#### How Would the Model Be Used?
**Primary Use:** Predictive early warning system

| Prediction | Business Action | Owner |
|-----------|-----------------|-------|
| High Churn Risk (>70%) | 1. Proactive outreach (email/call) | Customer Success |
| | 2. Offer incentives/premium features | Sales/Product |
| | 3. Schedule advisor consultation | Advisory Team |
| Medium Risk (40-70%) | 1. In-app engagement prompts | Product |
| | 2. Feature recommendations | Recommendation Engine |
| Low Risk (<40%) | Standard engagement | Standard Operations |

**Expected Business Impact:**
- Retain 20% of high-risk customers = 4-6% overall churn reduction
- Cost per retention action: ~$50-100
- LTV preservation: ~$2,000-5,000 per customer

---

### 4. Cost-Benefit Analysis: False Positives vs. False Negatives

#### Confusion Matrix Implications

| Outcome | Cost | Business Impact |
|---------|------|-----------------|
| **True Positive (TP)** | Retention action cost: $75 | Prevent $2,500 LTV loss ✓ Positive ROI |
| **False Positive (FP)** | Retention action cost: $75 | Customer already engaged; Cost-negative |
| **True Negative (TN)** | No cost | Customer retains naturally ✓ No intervention |
| **False Negative (FN)** | $0 intervention cost | **Lose $2,500 LTV + reputation risk** ✗ Most costly |

#### Cost-Benefit Decision Framework
```
Cost of FP (unnecessary retention action): $75
Cost of FN (missed churn opportunity): $2,500

Optimal Strategy: Tolerate higher FP rate to minimize FN rate
Recommended: Maximize Recall (TPR) over Precision for early stage
```

**Recommended Threshold:** Lower classification threshold to 0.35-0.40 (vs. default 0.50) to catch more at-risk customers

---

## STAGE 2-6: PLANNING THE PROJECT

### 5. Potential Feature Engineering Opportunities

#### From Customer Demographics
- **Age brackets:** Create risk cohorts (Gen-Z, Millennials, Gen-X patterns)
- **Income-income interactions:** Risk scores by income+age combination
- **Geographic clustering:** Regional churn patterns and economic indicators

#### From Account Data
- **Account maturity:** Days since account opening (newer accounts = higher churn)
- **Portfolio type encoding:** Conservative vs. Aggressive (behavioral proxy)
- **Account concentration:** % of assets in top holding
- **Rebalancing frequency:** Indicator of engagement level

#### From App Usage Data (Most Predictive)
- **Engagement score:** Logins/month trend (declining = churn signal)
- **Feature adoption:** Count of distinct features used monthly
- **Session quality:** Average session duration (short sessions = disengagement)
- **Activity decay:** Month-over-month login decline rate
- **Feature stickiness:** Repeat usage of specific features

#### Derived Features (High Value)
- **Engagement trajectory:** 30-day trend in logins (accelerating decline = high risk)
- **Portfolio growth:** % change in account balance (stagnation = concern)
- **Inactivity streak:** Consecutive days without login
- **Advisor interaction frequency:** Consultations/month (low = at risk)
- **Performance relative to benchmark:** Portfolio return vs. S&P 500

#### Temporal Features
- **Day of week effects:** Churn clustering
- **Seasonal patterns:** Q1/Q4 higher churn (tax season?)
- **Account age interactions:** Churn risk by tenure

---

### 6. Evaluation Metrics & Business KPIs

#### Primary Metric: **Recall (Sensitivity)**
```
Recall = TP / (TP + FN)
Target: 85%+ 

Why: Minimize false negatives (missed churners).
Interpretation: "Of all customers who will churn, we catch 85%"
```

#### Secondary Metrics (Ranked by Importance)

| Metric | Target | Why |
|--------|--------|-----|
| **Precision** | 50%+ | To minimize wasted retention actions |
| **F1-Score** | 0.65+ | Balance of precision and recall |
| **ROC-AUC** | 0.82+ | Model discrimination ability |
| **Specificity** | 75%+ | Minimize unnecessary actions on loyal customers |

#### Business Metrics
- **Retention Rate Improvement:** +20% on high-risk segment
- **Cost per Retention:** Keep <$75
- **LTV Impact:** Quantify customers saved × $2,500
- **False Positive Cost Ratio:** FP cost < Fn cost × 10

#### Model Performance Thresholds
```
✓ Acceptable: Recall ≥ 80%, Precision ≥ 45%
✓ Good: Recall ≥ 85%, Precision ≥ 55%
✓ Excellent: Recall ≥ 90%, Precision ≥ 65%
```

---

### 7. Deployment Architecture & Pattern

#### Recommendation: **Batch Processing (Weekly/Daily)**

**Why Batch Over Real-time:**
- Churn risk is not instantaneous; it evolves over weeks
- Cost: $50-100/customer for outreach requires strategic timing
- Operational: Customer Success team manages campaigns in batches

#### Deployment Pipeline

```
Data Pipeline (Daily/Weekly)
    ↓
Feature Engineering (Aggregations)
    ↓
Model Inference (Batch scoring of all active customers)
    ↓
Risk Segmentation (High/Medium/Low tiers)
    ↓
Action Assignment (Route to appropriate team)
    ↓
Campaign Execution (Email, SMS, calls)
    ↓
Outcome Tracking & Model Retraining
```

#### Implementation Architecture

**Option A: Scheduled Batch Job (RECOMMENDED)**
- **Frequency:** Daily or weekly batch scoring
- **Latency:** 24-48 hour delay acceptable
- **Scalability:** Process 100K customers in 1-2 hours
- **Tools:** Airflow/Prefect + cloud ML (AWS SageMaker, GCP Vertex)
- **Cost:** $200-500/month
- **Pros:** Simple, cost-effective, manageable
- **Cons:** Slight lag in predictions

**Option B: Real-time API**
- **Frequency:** Immediate scoring on login/action
- **Latency:** Sub-second
- **Scalability:** Handle 1000s of requests/second
- **Tools:** REST API + model serving (KServe, Seldon)
- **Cost:** $2,000-5,000/month
- **Pros:** Real-time personalization
- **Cons:** Engineering overhead, overkill for churn use case

#### Recommended: **BATCH (Daily/Weekly)**
- Set up daily/weekly batch job to score all active customers
- Updates churn probability in customer record
- Feeds into CRM and customer success workflows
- Retrains model weekly on new outcomes data

---

## Implementation Timeline

| Phase | Duration | Deliverable |
|-------|----------|-------------|
| **1. Data Exploration** | Week 1-2 | EDA, data quality report |
| **2. Feature Engineering** | Week 3-4 | Feature set, engineering pipeline |
| **3. Model Development** | Week 5-7 | Baseline → optimized models |
| **4. Evaluation & Validation** | Week 8 | A/B test plan, business case |
| **5. Deployment Setup** | Week 9-10 | Batch pipeline, integration |
| **6. Pilot Launch** | Week 11-12 | Limited rollout (10-20% customers) |
| **7. Full Production** | Week 13+ | 100% coverage, monitoring |

---

## Success Criteria

✓ Model achieves ≥85% Recall on holdout test set  
✓ Pilot cohort shows ≥15% improvement in retention vs. control  
✓ Prediction accuracy maintained over time  
✓ Deployment operational and monitored  
✓ Business case validated (ROI positive within 6 months)

---

## Risks & Mitigation

| Risk | Impact | Mitigation |
|------|--------|-----------|
| **Data quality issues** | Poor model | Data audit, validation rules |
| **Concept drift** | Declining accuracy | Weekly retraining, monitoring |
| **Low customer response** | Goal miss | A/B test retention offers |
| **Privacy concerns** | Regulatory | Anonymize features, transparency |
| **Over-servicing** | Fatigue | Cap outreach frequency |

---

## Next Steps

1. **Week 1:** Exploratory Data Analysis (EDA) on churn patterns
2. **Week 1-2:** Finalize feature list with Product & Analytics teams
3. **Week 3:** Build baseline logistic regression model
4. **Week 4-5:** Experiment with XGBoost, Random Forest
5. **Week 6:** Finalize evaluation metrics and business thresholds
6. **Week 7:** Deploy to batch production pipeline
7. **Week 8:** Launch pilot with Customer Success team
