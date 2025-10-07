# 🍽️ Restaurant Orders SQL Analysis

## 🧾 Overview
This project analyzes restaurant order data using **SQL** to uncover customer behavior, promo performance, and order trends.  
The dataset simulates **real-world online food delivery data** — tracking orders, cuisines, customers, and promotions.  
Through SQL analysis, this project answers key business questions that restaurant and food delivery companies face daily.

---

## 🎯 Objectives
- Identify which cuisines are most popular among customers  
- Analyze promo code effectiveness  
- Measure repeat vs. one-time customers  
- Track monthly order trends  
- Detect inactive customers for re-engagement  
- Evaluate overall business growth over time  
- Recognize loyal customers who place multiple orders  

---

## 🗂️ Database Schema

### 🧱 Table Creation
```sql
CREATE TABLE orders (
    Order_id VARCHAR(20),
    Customer_code VARCHAR(20),
    Placed_at DATETIME,
    Restaurant_id VARCHAR(10),
    Cuisine VARCHAR(20),
    Order_status VARCHAR(20),
    Promo_code_Name VARCHAR(20)
);
````

Each record represents one customer order including:

* **Customer_code** – unique customer identifier
* **Cuisine** – type of food ordered
* **Placed_at** – order timestamp
* **Promo_code_Name** – discount code used (if any)
* **Order_status** – delivered / cancelled

---

## 💼 Business Problems and SQL Solutions

### 1️⃣ Which cuisines are most popular among customers?

**Purpose:** Identify which cuisines drive the most demand to guide marketing and menu expansion.

```sql
SELECT Cuisine, COUNT(Order_id) AS total_order
FROM orders
WHERE Order_status = 'Delivered'
GROUP BY Cuisine
ORDER BY total_order DESC;
```

**Insight:** Lebanese and Italian cuisines receive the most orders — key areas for future promotion and new menu additions.

---

### 2️⃣ Which promo codes generate the highest number of successful deliveries?

**Purpose:** Assess effectiveness of marketing promotions and discount codes.

```sql
SELECT Promo_code_Name, COUNT(Order_id) AS total_delivered
FROM orders
WHERE Order_status = 'Delivered' AND Promo_code_Name IS NOT NULL
GROUP BY Promo_code_Name
ORDER BY total_delivered DESC;
```

**Insight:** `NEWUSER` and `FIRSTORDER` are the most effective in driving delivered orders — ideal for onboarding campaigns.

---

### 3️⃣ What percentage of customers place more than one order?

**Purpose:** Measure customer retention and loyalty.

```sql
SELECT
  customer_type,
  COUNT(*) AS total_customers,
  ROUND(100.0 * COUNT(*) / (SELECT COUNT(DISTINCT Customer_code) FROM orders WHERE Order_status = 'Delivered'), 2) AS pct_of_customers
FROM (
  SELECT Customer_code,
         CASE WHEN COUNT(*) > 1 THEN 'Repeat' ELSE 'One-Time' END AS customer_type
  FROM orders
  WHERE Order_status = 'Delivered'
  GROUP BY Customer_code
) AS t
GROUP BY customer_type;
```

**Insight:** Roughly half of customers are **repeat buyers**, a strong indicator of customer satisfaction and retention.

---

### 4️⃣ Are orders increasing or decreasing over months?

**Purpose:** Evaluate business performance and seasonality.

```sql
SELECT 
    DATE_FORMAT(Placed_at, '%Y-%m') AS month,
    COUNT(Order_id) AS total_orders
FROM orders
WHERE Order_status = 'Delivered'
GROUP BY month
ORDER BY month;
```

**Insight:** Orders consistently increase month-over-month from January to March 2025, suggesting growing customer engagement.

---

### 5️⃣ Inactive Customers (No Orders Recently)

**Purpose:** Identify churned or inactive customers for reactivation campaigns.

```sql
SELECT DISTINCT Customer_code
FROM orders
WHERE Customer_code NOT IN (
    SELECT DISTINCT Customer_code
    FROM orders
    WHERE Placed_at >= DATE_SUB('2025-03-31', INTERVAL 30 DAY)
);
```

**Insight:** Customers inactive in the last 30 days can be targeted with discounts or re-engagement emails.

---

### 6️⃣ How are orders trending over months?

**Purpose:** Compare monthly performance to visualize growth and consistency.

```sql
SELECT 
    DATE_FORMAT(Placed_at, '%Y-%m') AS month,
    COUNT(Order_id) AS total_orders
FROM orders
WHERE Order_status = 'Delivered'
GROUP BY month
ORDER BY month;
```

**Insight:** The restaurant platform shows **steady growth** in total orders each month — strong operational efficiency.

---

### 7️⃣ Which customers placed more than one order?

**Purpose:** Identify loyal customers for potential reward or membership programs.

```sql
SELECT 
    Customer_code,
    COUNT(Order_id) AS total_orders
FROM orders
GROUP BY Customer_code
HAVING COUNT(Order_id) > 1
ORDER BY total_orders DESC;
```

**Insight:** Customers like `MULTI_CUISINE_CUST` and `THIRD_ORDER_CUST1` are repeat buyers who could be incentivized for referrals or loyalty programs.

---

## 📊 Insights Summary

| Business Question  | Key Finding                                      |
| ------------------ | ------------------------------------------------ |
| Popular Cuisines   | Lebanese and Italian dominate orders             |
| Best Promo Codes   | `NEWUSER`, `FIRSTORDER` drive highest deliveries |
| Repeat Customers   | ~50% of customers are repeat buyers              |
| Growth Trend       | Orders increase monthly                          |
| Inactive Customers | Identified for re-engagement                     |
| Order Trends       | Consistent month-to-month growth                 |
| Loyal Customers    | Multiple high-frequency buyers spotted           |

---

## 🧠 Skills Demonstrated

* SQL Query Writing
* Aggregations & Filtering
* Subqueries
* Date & Time Functions
* Customer Segmentation
* Retention & Churn Analysis
* Business Insight Generation

Would you like me to generate this as a **ready-to-upload `README.md` file** (so you can drag it straight into your GitHub project folder)?
```
