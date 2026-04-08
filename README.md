# Sales Analytics BI Platform

## 📌 Overview

End-to-end Business Intelligence solution designed to analyze sales performance.
This project covers the full data lifecycle: ingestion, transformation, data modeling, and visualization.

The goal is to simulate a real-world BI workflow and demonstrate skills required for a **Power BI Developer / BI Analyst role**.

---

## 🏗️ Architecture

The project follows a layered data architecture:

```
Raw → Staging → Marts (Star Schema) → Power BI
```

### 🔹 Layers

* **Raw Layer**

  * Source data loaded from CSV using Python
  * Stored in PostgreSQL without transformations

* **Staging Layer**

  * Data cleaning and standardization using SQL
  * Fixed data types and normalized column names

* **Marts Layer**

  * Dimensional modeling (Star Schema)
  * Fact and dimension tables optimized for analytics

---

## 🛠️ Tech Stack

* **Python** (Pandas, SQLAlchemy)
* **PostgreSQL**
* **SQL**
* **Power BI**
* *(Optional)* VS Code, DBeaver

---

## 📊 Data Model

### ⭐ Fact Table

* `fact_sales`

  * order_id
  * customer_id
  * product_id
  * date_id
  * sales

### 📦 Dimension Tables

* `dim_customer`
* `dim_product`
* `dim_date`

This structure enables efficient querying and supports scalable BI reporting.

---

## 🔄 ETL Pipeline

### Extract

* CSV dataset loaded using Python

### Transform

* Cleaned column names (snake_case)
* Converted date fields
* Handled inconsistent source schema
* Removed duplicates in dimension tables

### Load

* Stored in PostgreSQL across:

  * `raw`
  * `staging`
  * `marts`

---

## 📈 Dashboard Features

* KPI Metrics:

  * Total Sales
  * Total Orders
  * Average Order Value
  * Sales YTD

* Time Analysis:

  * Sales trend over time
  * Year-over-Year comparison

* Business Breakdown:

  * Sales by Category
  * Sales by Segment

* Product Insights:

  * Top products by revenue (treemap)
  * Top product table

* Interactive Filters:

  * Category
  * Segment
  * Month

---

## 🔍 Key Insights

* Technology is the highest revenue-generating category
* Sales show consistent year-over-year growth
* The Consumer segment contributes the majority of sales
* Revenue is concentrated among a small set of high-performing products

---

## 🚀 How to Run the Project

### 1. Set up PostgreSQL

Create database:

```
bi_sales_project
```

### 2. Run ingestion

```
python main.py
```

### 3. Execute SQL scripts

* `staging/stg_superstore.sql`
* `marts/dim_customer.sql`
* `marts/dim_product.sql`
* `marts/dim_date.sql`
* `marts/fact_sales.sql`

### 4. Open Power BI

* Connect to PostgreSQL
* Load tables from `marts`
* Build relationships and visuals

---

## 📁 Project Structure

```
.
├── data/
├── python/
│   ├── ingestion/
│   ├── utils/
│   └── config/
├── sql/
│   ├── staging/
│   └── marts/
├── dashboards/
└── README.md
```

---

## 🎯 What This Project Demonstrates

* End-to-end BI pipeline design
* Data cleaning and transformation
* Dimensional modeling (star schema)
* DAX and Power BI development
* Business-oriented data analysis

---

## 📌 Future Improvements

* Add Airflow for orchestration
* Migrate to BigQuery (cloud warehouse)
* Implement incremental loading
* Add data quality tests

---

## 👤 Author

Built as part of a portfolio project to demonstrate real-world BI and Data Engineering skills.

## Images
<img width="1290" height="722" alt="image" src="https://github.com/user-attachments/assets/cb0d104a-1bc0-44f6-bc3a-ea969ed1abf9" />

