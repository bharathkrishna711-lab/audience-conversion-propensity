# Audience Conversion Propensity Modeling & Campaign Simulation

## 1. Project Overview

This project builds an end-to-end **audience conversion propensity model** to help marketing teams decide **which audience groups are more likely to convert after seeing an ad within 7 days**.

The focus is not just on building a model, but on **turning predictions into practical campaign decisions**.

---

## 2. Business Problem

Marketing teams run campaigns for different products (Books, Luxury, Fitness, etc.) and different audience segments.

Instead of showing ads equally to everyone, the goal is to answer:

> **Which audience cohorts should be prioritized because they are more likely to convert?**

This is solved using **propensity modeling**, which ranks audience cohorts by likelihood of conversion.

---

## 3. Dataset Description

Each row in the dataset represents:

> **One audience cohort, for one product category, measured over a 7-day campaign window.**

### Key Columns (Simple Meaning)

* `as_of_date` – Reference date when the 7-day campaign window starts
* `audience_segment` – Type of audience (e.g., Tech Shoppers, Book Lovers)
* `product_category` – Product being advertised
* `geo`, `device` – Campaign context
* `users_exposed` – Number of users shown the ad
* `impressions_7d` – Total ad impressions over the next 7 days
* `clicks_7d`, `site_visits_7d` – Engagement signals
* `add_to_cart_7d` – Purchase intent signal
* `conversion_rate_7d` – Actual 7-day conversion outcome

All `*_7d` metrics are measured **in the 7 days after `as_of_date`**.

---

## 4. Feature Engineering

To improve model learning, additional features were created:

* Clicks per user
* Add-to-cart rate
* Frequency–recency ratio
* Seasonal engagement interaction

These features normalize raw counts and better represent user behavior.

---

## 5. Target Definition

The model predicts whether a cohort belongs to the **top 20% of converters**.

* Cohorts in the top 20% of `conversion_rate_7d` → labeled as 1
* Remaining cohorts → labeled as 0

This framing supports **ranking and prioritization**, which is how real campaigns operate.

---

## 6. Exploratory Data Analysis (EDA)

EDA was performed to:

* Understand feature distributions
* Identify relationships with conversion rate
* Detect data quality issues

Key insights:

* Higher engagement (site visits, add-to-cart) aligns with higher conversion
* Audience segments and product categories show different conversion patterns
* Conversion rate values are clean and realistic

---

## 7. Modeling Approach

* Model: **Logistic Regression**
* Implemented using a **scikit-learn Pipeline**
* Pipeline includes preprocessing (numeric scaling, categorical encoding) and the model

The full pipeline is trained and saved to ensure consistency during inference.

---

## 8. Model Output

The model outputs a **propensity score** between 0 and 1 for each cohort:

> Higher score = more likely to be a high-converting cohort

These scores are used for ranking, not causal inference.

---

## 9. Campaign Simulation

The trained pipeline is reused to simulate campaigns:

* Score unseen cohorts
* Aggregate predictions by audience segment
* Compare predicted propensity with actual conversion rates

This produces actionable tables such as:

> "Which audience segments should we target first?"

---

## 10. Key Result

Audience segments like **Tech Shoppers, Luxury Shoppers, and Fitness Enthusiasts** show higher predicted propensity and strong observed conversion rates.

Lower-performing segments can be deprioritized or targeted differently.

---

## 11. Project Structure

```
Audience-Conversion-Propensity/
│
├── notebooks/
│   ├── eda.ipynb
│   ├── modeling.ipynb
│   └── campaign_simulation.ipynb
│
├── models/
│   └── logit_pipeline.joblib
│
├── data/
│   └── raw/
│
├── README.md
```

---

## 12. Key Takeaway

This project demonstrates how to:

* Build a realistic marketing ML model
* Handle biased, real-world campaign data
* Translate model predictions into business decisions

The final outcome is a **decision-support tool** for campaign prioritization, not just a model.
