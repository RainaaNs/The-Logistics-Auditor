# 📦 The "Last Mile" Logistics Auditor
**Client:** Veridi Logistics (Global E-Commerce Aggregator)

---

## A. Executive Summary

This project audits last-mile delivery performance across orders from the Olist Brazilian e-commerce dataset. The vast majority of orders (~91.6%) were delivered on time, but a meaningful tail of **~6.7% of orders** arrived late (1–5 days) or super late (5+ days). Regional analysis revealed that states in Brazil's Northeast - particularly Alagoas (AL), Maranhão (MA), and Piauí (PI) - consistently showed the highest late-delivery rates, though this was not strongly explained by distance from São Paulo alone. Most strikingly, customer review scores dropped sharply with delivery delays: on-time orders averaged a score of **4.29/5**, while super-late orders averaged just **1.74/5**, confirming that delivery reliability is a primary driver of customer satisfaction.

---

## B. Project Links

- **Notebook:** [Google Colab - the_logistics_auditor.ipynb](https://colab.research.google.com/drive/1BO5peqDecgsc4JkUM_tJJogTQW76v03j?usp=sharing)
- **Dashboard:** *https://naaapp-master-9xph6xztpwyvbebvkczcbe.streamlit.app/*
- **Presentation:** *https://ugedugh-my.sharepoint.com/:p:/g/personal/nsaaddo_st_ug_edu_gh/IQC7G6QPo8fTSYlmwIrMrwKkAd7YL1PWoN0F17xDlHR7rW0?e=04xj2f*
- **Video Walkthrough (Optional):** *https://youtu.be/UH2mNOLQ3Ys*


---

## C. Technical Explanation

### Data Cleaning

The raw dataset started with **110,000 + orders** across three source files: orders, reviews, and customer locations. The following cleaning steps were applied to arrive at the final working dataset:

1. **Duplicate reviews:** Some `order_id` values had a few review entries. These were resolved by averaging `review_score` per order and then dropping duplicates on `order_id`, ensuring a clean one-to-one order–review mapping.

2. **Merging:** Orders were joined left to reviews on `order_id`, then joined left to customer locations on `customer_id`. Both merges preserved the original order count, confirming no fan-out inflation.

3. **Dropping high-missingness columns:** `review_comment_title` (~88% missing) and `review_comment_message` (~59% missing) were dropped as they exceeded the 50% missingness threshold.

4. **Retaining rows with low missingness:** Columns with under 3% missing values (e.g. `order_delivered_customer_date` at ~2.98%) were kept as-is rather than dropping rows - preserving data volume while accepting minor missingness in non-critical fields.

5. **Date conversion:** `order_delivered_customer_date` and `order_estimated_delivery_date` were converted from string objects to `datetime64` to enable date arithmetic.

6. **Filtering invalid order statuses:** Orders with `order_status` of `"canceled"` or `"unavailable"` were excluded, as they have no meaningful delivery outcome to audit.

### Delivery Status Classification

A `calculate_delay()` function computed `days_diff` (actual delivery minus estimated delivery) and classified each order into three buckets using `pd.cut()`:


### Candidate's Choice - Story 6: Product Category Late Rate Analysis

As the "Candidate's Choice" addition, product category data was brought in by merging `olist_order_items_dataset.csv` with `olist_products_dataset.csv` and the `product_category_name_translation.csv` lookup table. This enriched the master dataset with English-language product category labels, enabling a new analytical angle: **which product categories suffer the worst on-time delivery performance?**

This was surfaced in the dashboard as an interactive horizontal bar chart (Tab 3: Category Analysis) showing the Top 10 worst-performing categories by late delivery rate, along with a full sortable category table. The insight here is operationally valuable - if specific categories skew late, it may point to supplier-side delays, fulfillment centre bottlenecks, or category-specific packaging challenges that Veridi can address directly with sellers.

---

## 🛑 Pre-Submission Checklist

### 1. Repository & Code Checks
- [ ] **My GitHub Repo is Public.** (Open the link in a Private/Incognito window to verify).
- [ ] **I have uploaded the `.ipynb` notebook file.**
- [ ] **I have ALSO uploaded an HTML or PDF export** of the notebook.
- [ ] **I have NOT uploaded the massive raw dataset.** (Use `.gitignore` or just don't commit the CSV).
- [ ] **My code uses Relative Paths.**

### 2. Deliverable Checks
- [ ] **My Dashboard link is publicly accessible.** (No login required).
- [ ] **My Presentation link is publicly accessible.** (Permissions set to "Anyone with the link can view").
- [ ] **I have updated this `README.md` file** with my Executive Summary and technical notes.

### 3. Completeness
- [ ] I have completed **User Stories 1–4**.
- [ ] I have completed the **"Candidate's Choice"** challenge and explained it in the README.

**✅ Only when you have checked every box above, proceed to the [Official Submission Form](https://forms.office.com/e/heitZ9PP7y).**
