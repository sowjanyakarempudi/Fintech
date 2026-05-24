# NextGen Wealth: Customer Churn Reduction
## AI-Driven ML Project Presentation

---

# SLIDE 1: TITLE SLIDE
## NextGen Wealth
### Reducing Customer Churn by 20% with AI

**Head of Data Science**  
Presented: May 24, 2026

---

# SLIDE 2: BUSINESS CONTEXT
## The Challenge

- **Company:** NextGen Wealth - FinTech robo-advisory platform
- **Problem:** High customer acquisition BUT high customer churn
- **Business Goal:** Reduce churn by 20% in next fiscal year
- **Opportunity:** Use AI/ML to identify at-risk customers before they leave

**Why This Matters:**
- Customer LTV: ~$2,500 per retained customer
- Current churn rate: ~15-20% annually (industry: 10%)
- Potential revenue impact: $2-5M if 20% improvement achieved

---

# SLIDE 3: DATA AVAILABLE
## What We Have to Work With

### Customer Demographics
- Age, income bracket, city/state
- Account tenure, risk profile preference

### Account Data
- Account balance, portfolio type (conservative/aggressive)
- Portfolio concentration, rebalancing patterns

### App Usage Data
- Monthly logins, features used, session duration
- Engagement trends, advisor interaction frequency

### Historical Data
- Complete list of churned customers (12-24 months)
- Enables supervised learning approach

---

# SLIDE 4: THE ML PROBLEM
## Problem Definition

**Task Type:** Binary Classification

**Target Label:** `churned` (Yes/No)
- Definition: Customer closes account by personal request
- Prediction window: 30/60/90 days ahead
- Exclude: Fraud closures, dormant accounts, dispute cases

**Learning Approach:** Supervised Learning
- Training data: Historical customers + churn outcomes
- Enables: Predictive early warning system

```
Customer Features → ML Model → Churn Probability
                      ↓
              (High Risk / Medium / Low Risk)
```

---

# SLIDE 5: BUSINESS ACTION FRAMEWORK
## How Will We Use the Model?

| Risk Level | Churn Probability | Business Action | Owner |
|-----------|-------------------|-----------------|-------|
| **HIGH** | >70% | • Proactive outreach | Customer Success |
| | | • Offer incentives/premium features | Sales |
| | | • Schedule advisor consultation | Advisory Team |
| **MEDIUM** | 40-70% | • In-app engagement prompts | Product |
| | | • Feature recommendations | AI/Recommendation |
| **LOW** | <40% | • Standard engagement | Operations |

**Expected Impact:**
- Retain 20% of high-risk customers = 4-6% overall churn reduction
- Cost per retention action: $50-100
- ROI: 25:1 (LTV $2,500 vs. cost $100)

---

# SLIDE 6: COST-BENEFIT ANALYSIS
## False Positives vs. False Negatives

### The Math
```
Cost of False Positive (unnecessary action): $75
Cost of False Negative (missed churner):     $2,500

Ratio: FN is 33x MORE EXPENSIVE than FP
```

### Decision Framework

| Outcome | Probability | Cost | Action |
|---------|------------|------|--------|
| **True Positive** | Save customer | $75 | ✓ Positive ROI |
| **False Positive** | Unnecessary action | $75 | Tolerable cost |
| **False Negative** | Lose customer | $2,500 | ✗ Prevent at all costs |

### Strategy
**→ Optimize for RECALL (minimize false negatives)**
- Lower classification threshold to 0.35-0.40
- Tolerate higher false positive rate
- Better to over-engage than miss churners

---

# SLIDE 7: FEATURE ENGINEERING
## What Signals Predict Churn?

### Engagement Signals (MOST PREDICTIVE)
- 📉 **Login trend:** Declining monthly logins = strong churn signal
- ⏱️ **Session duration:** Shorter sessions = disengagement
- 🔄 **Inactivity streak:** Days without login
- 📊 **Activity decay:** Month-over-month login decline rate

### Account Signals
- 🆕 **Account age:** Newer accounts = higher churn risk
- 💼 **Portfolio type:** Conservative vs. Aggressive behavior
- 💰 **Account growth:** Stagnation indicates lack of engagement
- ⚖️ **Rebalancing frequency:** Behavioral proxy for engagement

### Demographic Signals
- 👤 **Age cohort:** Gen-Z/Millennials vs. Gen-X patterns
- 💵 **Income bracket:** Risk varies by income level
- 📍 **Geography:** Regional economic factors

### Derived High-Value Features
- **Engagement trajectory:** 30-day login trend (most important!)
- **Advisor interaction frequency:** Consultations/month
- **Performance vs. benchmark:** Portfolio return vs. S&P 500
- **Feature stickiness:** Repeat usage of key features

---

# SLIDE 8: EVALUATION METRICS
## How Will We Measure Success?

### Primary Metric: RECALL
```
Recall = TP / (TP + FN) = "Of all customers who churn, 
         what % do we catch?"

TARGET: ≥ 85%
Meaning: Catch at least 85% of actual churners
```

### Secondary Metrics

| Metric | Target | Why |
|--------|--------|-----|
| **Precision** | ≥50% | Minimize wasted retention actions |
| **F1-Score** | ≥0.65 | Balanced performance |
| **ROC-AUC** | ≥0.82 | Model discrimination |
| **Specificity** | ≥75% | Minimize over-engagement |

### Business KPIs
- **Retention improvement:** +20% on high-risk segment
- **Cost per retention:** <$75 per customer
- **LTV impact:** Customers saved × $2,500
- **Model efficiency:** Run time <2 hours for 100K customers

---

# SLIDE 9: DEPLOYMENT STRATEGY
## How Will We Deploy?

### Recommendation: BATCH PROCESSING ✓

```
Daily/Weekly Batch Job
        ↓
Score ALL active customers (100K+)
        ↓
Segment into risk tiers (High/Med/Low)
        ↓
Send to CRM & Customer Success workflows
        ↓
Execute targeted retention campaigns
        ↓
Track outcomes & retrain model
```

### Why Batch vs. Real-Time?

| Aspect | Batch | Real-Time |
|--------|-------|-----------|
| **Frequency** | Daily/Weekly | Immediate |
| **Latency** | 24-48 hours | Sub-second |
| **Cost** | $200-500/mo | $2,000-5,000/mo |
| **Complexity** | Low | High |
| **Scalability** | 100K customers/2hrs | 1000s req/sec |
| **Best For** | Churn (evolves over weeks) | Real-time personalization |

**→ BATCH is optimal for churn use case**

---

# SLIDE 10: IMPLEMENTATION TIMELINE
## 12-Week Rollout Plan

| Week | Phase | Deliverable | Owner |
|------|-------|-------------|-------|
| 1-2 | Data Exploration | EDA, data quality audit | Data Engineering |
| 3-4 | Feature Engineering | Feature pipeline, engineering code | Data Science |
| 5-7 | Model Development | Baseline → optimized models (XGBoost) | Data Science |
| 8 | Validation & Testing | A/B test plan, business case | Analytics |
| 9-10 | Deployment Setup | Batch pipeline, monitoring | ML Engineering |
| 11-12 | Pilot Launch | Limited rollout (10-20% customers) | Product/CS |
| 13+ | Full Production | 100% coverage, ongoing monitoring | Operations |

---

# SLIDE 11: SUCCESS CRITERIA
## How We Know It Worked

### Model Performance
✓ Achieves ≥85% Recall on holdout test set  
✓ ≥50% Precision (acceptable false positive rate)  
✓ Maintains accuracy over time (concept drift monitored)

### Business Impact
✓ Pilot cohort shows ≥15% retention improvement vs. control  
✓ Positive ROI within 6 months  
✓ Engaged customer success team (adoption >80%)

### Operational
✓ Batch pipeline runs reliably (<2 hours/day)  
✓ Model retraining automated (weekly)  
✓ Alerts set up for performance degradation

---

# SLIDE 12: RISKS & MITIGATION
## What Could Go Wrong?

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|-----------|
| **Poor data quality** | Low model accuracy | Medium | Data audit, validation rules |
| **Concept drift** | Model degrades over time | Medium | Weekly retraining, monitoring |
| **Low customer response** | Goal not met | Medium | A/B test retention offers |
| **Privacy concerns** | Regulatory issues | Low | Feature anonymization, transparency |
| **Over-engagement** | Customer fatigue | Medium | Cap outreach frequency |

---

# SLIDE 13: RESOURCE REQUIREMENTS
## What We Need

### Team
- 1 Lead Data Scientist (full-time)
- 1 ML Engineer (full-time)
- 1 Data Engineer (0.5 FTE)
- 1 Analytics PM (0.25 FTE)

### Budget
- Compute/ML tools: $2,000-3,000/month
- Personnel: ~$400K annualized (loaded cost)
- Total 12-week investment: ~$150K

### Expected ROI
- Cost: $150K (development) + $10K/month (operations)
- Benefit: 20% churn reduction = $2-5M annual value
- **Payback period: 1-2 months**

---

# SLIDE 14: NEXT IMMEDIATE STEPS
## Week 1 Action Items

### Data Team
1. ✓ Conduct EDA on customer demographics & churn patterns
2. ✓ Validate data quality (missing values, outliers)
3. ✓ Finalize feature list with stakeholders
4. ✓ Set up data pipeline infrastructure

### Business Stakeholders
1. ✓ Align on churn definition (confirm exclusions)
2. ✓ Identify pilot customer segment (10-20%)
3. ✓ Plan retention offer testing & messaging
4. ✓ Schedule weekly sync meetings

### Success Metrics
- Week 2: EDA report complete
- Week 3: Feature engineering pipeline ready
- Week 4: Baseline model (logistic regression) trained
- Week 5-6: Advanced models (XGBoost) competing

---

# SLIDE 15: QUESTIONS & DISCUSSION

## Key Takeaways

1. **Problem:** Binary classification - predict which customers will churn
2. **Strategy:** Optimize for Recall (85%+) to minimize false negatives
3. **Features:** Engagement trajectory is most predictive signal
4. **Deployment:** Batch processing daily/weekly (cost-effective)
5. **Timeline:** 12 weeks to pilot, then full rollout
6. **Impact:** 20% churn reduction = $2-5M annual value

## Let's Discuss
- Data quality & timeline concerns?
- Business action framework alignment?
- Resource/budget questions?
- Next steps & ownership?

---

# APPENDIX: TECHNICAL DETAILS

## Model Candidates (Week 5-6)

### 1. Logistic Regression (Baseline)
- Pros: Fast, interpretable, low compute
- Cons: Limited non-linearity
- Expected AUC: 0.75-0.78

### 2. Random Forest
- Pros: Feature importance, handles non-linearity
- Cons: Black box, slower inference
- Expected AUC: 0.82-0.85

### 3. XGBoost (RECOMMENDED)
- Pros: State-of-art, fast training/inference, feature importance
- Cons: Hyperparameter tuning required
- Expected AUC: 0.85-0.90

### 4. Neural Network
- Pros: High capacity, non-linear patterns
- Cons: Requires large data, harder to interpret
- Expected AUC: 0.85-0.92

**Recommendation:** XGBoost balances performance, speed, and interpretability

---

## Hyperparameter Tuning Strategy

```
Cross-Validation: 5-fold stratified
Optimization: Bayesian search (100 trials)
Metrics: Maximize F1-score, ensure Recall ≥ 85%
Early Stopping: Monitor validation Recall

Key parameters:
- learning_rate: 0.01-0.1
- max_depth: 4-8
- subsample: 0.7-1.0
- colsample_bytree: 0.7-1.0
```

---

## Monitoring & Retraining

### Automated Monitoring
- **Daily:** Check model inference latency
- **Weekly:** Calculate Precision/Recall on recent predictions
- **Monthly:** Compare prediction accuracy vs. actual outcomes

### Retraining Triggers
- Recall drops below 80%
- Precision drops below 45%
- Data distribution shift detected
- New features available

### Retraining Frequency
- **Default:** Weekly (automated)
- **Ad-hoc:** When triggers activated
- **Full retrain:** Every quarter (feature engineering review)

---

## Privacy & Compliance

### Data Handling
- De-identify PII in model features
- Use aggregated engagement metrics (not session-level)
- Comply with CCPA/GDPR retention policies

### Transparency
- Customers opt-out of targeting if desired
- Clear communication about retention offers
- No discriminatory feature usage (protected classes)

### Governance
- Model card documentation
- Monthly accuracy audits
- Stakeholder review quarterly
