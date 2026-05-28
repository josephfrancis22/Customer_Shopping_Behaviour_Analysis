# Customer_Shopping_Behaviour_Analysis


A full-stack data analytics project that explores customer purchasing patterns through **Python-based EDA**, **PostgreSQL queries**, and an interactive **Power BI dashboard**.

---

##  Project Overview

This project analyses a retail customer dataset to uncover key insights around revenue, purchasing habits, discount usage, product preferences, and customer segmentation. The workflow spans data ingestion and cleaning in Python, analytical querying in SQL, and visual storytelling in Power BI.

---

##  Repository Structure

```
Customer_Shopping_Behaviour_Analysis/
│
├── Customer_Shopping_Behaviour_Analysis.ipynb   # EDA, feature engineering & DB upload
├── customer_behavior_sql_queries.sql            # 10 analytical SQL queries
├── customer_behavior_dashboard.pbix             # Power BI interactive dashboard
└── README.md
```

---

##  Dataset

**File:** `customer_shopping_behavior.csv`

The dataset contains transactional and demographic data for retail customers, including:

| Column | Description |
|---|---|
| `customer_id` | Unique customer identifier |
| `age` | Customer age |
| `gender` | Male / Female |
| `category` | Product category |
| `item_purchased` | Specific product bought |
| `purchase_amount` | Purchase value in USD |
| `review_rating` | Product rating (1–5) |
| `shipping_type` | Standard, Express, etc. |
| `discount_applied` | Whether a discount was used (Yes/No) |
| `subscription_status` | Subscribed / Not Subscribed |
| `frequency_of_purchases` | Purchase frequency (Weekly, Monthly, etc.) |
| `previous_purchases` | Count of prior purchases |

---

##  Tech Stack

| Tool | Purpose |
|---|---|
| **Python** (Pandas) | Data cleaning, feature engineering |
| **Google Colab** | Development environment |
| **SQLAlchemy + psycopg2** | Python–PostgreSQL connection |
| **PostgreSQL** (Neon) | Cloud database & SQL analysis |
| **Power BI** | Interactive dashboard |

---

##  Data Cleaning & Feature Engineering

Performed in `Customer_Shopping_Behaviour_Analysis.ipynb`:

- **Null handling** — Missing `review_rating` values filled with the **median rating per category** (group-wise imputation)
- **Column standardisation** — Headers converted to lowercase and snake_case; `purchase_amount_(usd)` renamed to `purchase_amount`
- **Duplicate column removal** — `promo_code_used` was found to be identical to `discount_applied` and was dropped
- **Feature: `age_group`** — Customers segmented into four quartile-based age bands using `pd.qcut`:
  - Young Adult → Adult → Middle-aged → Senior
- **Feature: `purchase_frequency_days`** — Frequency labels mapped to numeric day intervals (e.g., Weekly → 7, Monthly → 30, Annually → 365)

---

## 🗄️ Database Setup

The cleaned DataFrame was pushed to a **PostgreSQL database hosted on Neon** via SQLAlchemy:

```python
from sqlalchemy import create_engine

engine = create_engine("postgresql://<user>:<password>@<host>/customer_Analysis?sslmode=require")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

All 10 SQL queries run against the `customer` table in this database.

---

## 🔍 SQL Analysis — Key Questions

Ten business questions were answered using PostgreSQL:

| # | Question | Technique |
|---|---|---|
| Q1 | Total revenue by gender | `GROUP BY`, `SUM` |
| Q2 | Discount users who still spent above average | Scalar subquery, `WHERE` filter |
| Q3 | Top 5 products by average review rating | `AVG`, `ROUND`, `ORDER BY`, `LIMIT` |
| Q4 | Average purchase amount: Standard vs Express shipping | Filtered `GROUP BY` |
| Q5 | Do subscribers spend more? (avg spend & total revenue) | `COUNT`, `AVG`, `SUM` comparison |
| Q6 | Top 5 products by discount application rate | Conditional `SUM`, percentage calculation |
| Q7 | Customer segmentation: New / Returning / Loyal | `CTE`, `CASE WHEN` |
| Q8 | Top 3 products per category by order volume | `CTE`, `ROW_NUMBER()`, Window function |
| Q9 | Repeat buyers (>5 purchases) and subscription tendency | Filtered `GROUP BY` |
| Q10 | Revenue contribution by age group | `SUM`, `GROUP BY` on engineered column |

---

## 📈 Power BI Dashboard

The `customer_behavior_dashboard.pbix` file contains an interactive dashboard with visuals covering:

-  Revenue breakdown by gender and age group
-  Top-performing product categories and items
-  Subscription vs non-subscription spending comparison
-  Discount usage patterns across products
-  Shipping type preference and purchase value
-  Customer loyalty segmentation (New / Returning / Loyal)

---

##  Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/josephfrancis22/Customer_Shopping_Behaviour_Analysis.git
cd Customer_Shopping_Behaviour_Analysis
```

### 2. Run the notebook
Open `Customer_Shopping_Behaviour_Analysis.ipynb` in Google Colab or Jupyter. Upload `customer_shopping_behavior.csv` and run all cells.

### 3. Run SQL queries
Connect to your PostgreSQL instance and execute `customer_behavior_sql_queries.sql` against the `customer` table.

### 4. View the dashboard
Open `customer_behavior_dashboard.pbix` in **Power BI Desktop**.

---

##  Key Insights

- **Subscribed customers** generate significantly higher revenue despite similar average spend per transaction
- **Discounted purchases** do not necessarily lead to lower spending — a segment of discount users still spend above the average
- **Loyal customers** (11+ previous purchases) form the backbone of repeat revenue
- **Age group** and **product category** are strong predictors of revenue contribution

---

##  Author

**Joseph Francis**
- GitHub: [@josephfrancis22](https://github.com/josephfrancis22)

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
