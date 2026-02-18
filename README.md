# AI Merchant Transaction Anomaly Detector

---

## The Problem

Small and mid-size merchants lose billions annually to fraudulent transactions, but most don't have the tools or expertise to detect suspicious patterns in their sales data. By the time they notice something is wrong, a chargeback notification, an unfamiliar charge on their statement the damage is done.

Traditional rule-based monitoring systems suffer from:
- **High false positives** for large or seasonal merchants
- **Missed signals** for smaller merchants with unique patterns
- **Zero context** — they flag transactions but never explain *why*, leaving merchants confused and unable to act

---

## The Solution

A prototype anomaly detection tool that:

1. **Learns what "normal" looks like** for each merchant by analyzing their transaction history
2. **Flags transactions that don't fit the pattern** using an Isolation Forest machine learning model
3. **Uses AI to generate plain-English explanations** of why each transaction is suspicious — so any seller can understand and act on the alert, no data team required

---

## How It Works

### Data & Feature Engineering
- Synthetic dataset of **17,800+ transactions** across 5 merchants with realistic behavior profiles
- Engineered features that capture each merchant's unique baseline:
  - **Amount z-score** — how far is this transaction from the merchant's typical range?
  - **Amount ratio** — transaction amount vs. merchant's median
  - **Hour deviation** — is this happening during normal operating hours?
  - **After-hours flag** — binary indicator for transactions outside business hours
  - **Transaction velocity** — are transactions coming in faster than usual?

### Machine Learning Model
- **Isolation Forest** (unsupervised) — isolates anomalies without requiring labeled fraud data
- Trained on per-merchant transaction patterns
- Detected **55% of injected anomalies** at a 3% flag rate across three fraud types:
  - Unusually large transactions
  - After-hours activity
  - Rapid-fire card testing

### AI-Powered Explanations
This is where the product comes to life. Raw anomaly flags aren't useful to a seller, they need to understand **why** and **what to do**.

**Rule-based layer:** Generates structured alerts based on feature values (e.g., "This $474.71 transaction is 55x your typical sale of $8.50 and occurred at 4:00 AM, outside your normal hours")

**LLM-powered layer:** Connects to an AI API to generate richer, more contextual explanations for edge cases where multiple factors combine in unusual ways, making alerts feel like they were written by a human analyst.

---

## Who This Helps

| User | How They Use It |
|------|----------------|
| **Merchants / Sellers** | Receive plain-English alerts about suspicious transactions and take action before chargebacks hit |
| **Risk & Operations teams** | Triage flagged merchants faster with ranked anomaly scores and explanations |
| **Product teams** | Understand merchant behavior changes after launches, migrations, or policy updates |

---

## Product Success Metrics

- **Alert precision**: percent of flagged transactions confirmed as suspicious
- **False positive rate**: keep below a defined threshold to avoid merchant fatigue
- **Merchant action rate**: percent of alerts that lead to a review or action within 24 hours
- **Chargeback reduction**: change in chargeback rate over 60-90 days after rollout
- **Alert engagement**: open rate and follow-up actions on alerts
  
---

## Product Roadmap

### Phase 1 — MVP (Weeks 1-4)
- Integrate with the payment platform's transaction data pipeline
- Deploy Isolation Forest model on per-merchant transaction streams
- Ship basic alerts with rule-based explanations
- **Success metrics:** alert engagement rate, false positive rate, seller-reported fraud reduction

### Phase 2 — AI-Native Alerts (Weeks 5-8)
- Integrate LLM-powered explanations for nuanced, context-aware alerts
- A/B test LLM vs. rule-based explanations: measure click-through, satisfaction, action rate
- Add severity tiers with different notification channels (in-app vs. email vs. SMS)

### Phase 3 — Feedback Loop & Personalization (Weeks 9-12)
- Add "Was this helpful?" feedback to improve precision over time
- Fine-tune the model per merchant category using seller feedback
- Build seller-facing controls: sensitivity settings, quiet hours, trusted customer lists

---

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| **Isolation Forest over deep learning** | Unsupervised, interpretable, works without labeled fraud data, right for an MVP where simplicity and trust matter more than marginal accuracy |
| **Per-merchant baselines** | A $500 transaction is normal for a jewelry store but suspicious for a coffee shop. Context-aware detection reduces false positives |
| **AI explanations over raw scores** | Merchants aren't data scientists. Plain-English alerts drive action; anomaly scores don't |
| **Human-in-the-loop** | The system surfaces signals but doesn't auto-block transactions. Trust and merchant experience come first |

---

## Risks & Tradeoffs

- **Sensitivity vs. noise**: higher sensitivity catches more fraud but can overwhelm merchants
- **Cold start merchants**: limited history can reduce accuracy; requires conservative defaults
- **LLM cost and latency**: AI explanations add value but require careful rate and cost control
- **Merchant trust**: explanations must be consistent and verifiable to avoid confusion

---

## Repository Structure

```
merchant-anomaly-detector/
  merchant_anomaly_detector.ipynb   # Full prototype: data, EDA, model, AI explanations
  README.md                         # Project overview and product rationale
```

---
