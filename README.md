# 📊 Ecommerce Business Insights Analysis

Analyzed 99K+ e-commerce orders from the Olist Brazilian marketplace using SQL and Power BI to uncover revenue trends, customer purchasing behavior, seller performance, delivery efficiency, and payment insights.

---

## 🛠 Tools Used

PostgreSQL 18 • SQL (JOINs, CTEs, Window Functions, LAG, CASE WHEN) • Power BI • Excel

---

## 📌 Project Objective

The goal of this project was to perform end-to-end business analysis on a large-scale e-commerce dataset and generate actionable insights around sales growth, customer retention, delivery performance, and operational efficiency.

---

## 📂 Dataset Overview

| Property | Detail |
|----------|--------|
| Source | Olist Brazilian E-Commerce Dataset (Kaggle) |
| Orders | 99,441 |
| Customers | 96,096 unique customers |
| Time Period | 2016 – 2018 |
| Tables Used | customers, orders, order_items, payments, products, reviews |

---

## 🔍 Business Questions Solved

- Which product categories generated the highest revenue?
- Which states and cities contributed the most sales?
- How did monthly revenue grow over time?
- Which sellers generated the highest revenue?
- How do delayed deliveries affect customer review scores?
- Which payment methods are most preferred by customers?
- What percentage of customers are repeat buyers?
- Which states have the slowest average delivery times?
- How did cancellation rates change month-over-month?

---

## 📈 Key Metrics

| Metric | Value |
|--------|-------|
| Total Revenue | R$ 15.84M |
| Total Orders | 99K |
| Total Customers | 96K |
| Average Order Value | R$ 160.58 |

---

## 📊 Key Findings

- Delayed deliveries consistently received lower customer review scores
- Credit cards dominated both revenue contribution and order volume
- São Paulo generated the highest overall revenue by a significant margin
- Repeat customers represented a smaller customer segment but contributed disproportionately high revenue
- Monthly revenue showed strong growth throughout 2017 before stabilizing in 2018
- Delivery performance varied significantly across different states

---

## 💻 SQL Highlight — Month-over-Month Revenue Growth

```sql
SELECT *,
    ROUND(
        (total_revenue - prv_mon_rev) * 100.0 / prv_mon_rev,
        2
    ) AS growth_percentage
FROM (
    SELECT
        month,
        total_revenue,
        LAG(total_revenue) OVER (ORDER BY month) AS prv_mon_rev
    FROM (
        SELECT
            DATE_TRUNC('MONTH', o.order_purchase_timestamp) AS month,
            SUM(oi.price + oi.freight_value) AS total_revenue
        FROM orders o
        JOIN order_items oi
            ON o.order_id = oi.order_id
        WHERE o.order_status = 'delivered'
        GROUP BY DATE_TRUNC('MONTH', o.order_purchase_timestamp)
    ) t
) t2;
