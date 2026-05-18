# 📊 End-to-End BI Sales Analytics

Complete Business Intelligence solution built on the **Superstore Sales dataset** — from raw CSV ingestion into PostgreSQL, through SQL staging transformations and star schema modeling, to a Power BI dashboard with business KPIs.

---

## 🏗️ Architecture

```
data/raw/train.csv  (Superstore Sales — 9,000+ records, 2015–2018)
        │
        ▼ Python + Pandas + SQLAlchemy
┌──────────────────────┐
│   raw.superstore_raw │  Raw layer in PostgreSQL
│   (schema: raw)      │  Column names normalized
└──────────────────────┘
        │
        ▼ SQL transformation
┌──────────────────────┐
│ staging.stg_superstore│  Cleaned staging layer
│ (schema: staging)    │  Date casting, type cleaning
└──────────────────────┘
        │
        ▼ Star Schema modeling
┌──────────────────────┐
│  dim_customers       │  Dimensions
│  dim_products        │
│  dim_date            │
│  fact_sales          │  Fact table
└──────────────────────┘
        │
        ▼ Power BI
┌──────────────────────┐
│  Sales Dashboard     │  KPIs, trends, segmentation
└──────────────────────┘
```

---

## ⚙️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3 + Pandas | CSV ingestion and column normalization |
| SQLAlchemy | Database connection and ORM |
| PostgreSQL | Data warehouse (raw + staging schemas) |
| SQL | Staging transformations and type casting |
| Power BI | Dashboard and KPI visualization |

---

## 📁 Project Structure

```
end-to-end-bi-sales-analytics/
├── main.py                        # Entry point
├── python/
│   ├── Orquestador.py             # Pipeline orchestrator
│   ├── config/
│   │   └── db_config.py          # PostgreSQL connection config
│   ├── ingestion/
│   │   └── load_raw_data.py      # CSV → raw.superstore_raw
│   └── utils/
│       └── db.py                 # SQLAlchemy engine factory
├── sql/
│   └── staging/
│       └── stg_superstore.sql    # Raw → staging transformation
├── data/
│   └── raw/
│       └── train.csv             # Superstore Sales dataset
├── dashboards/
│   └── Sales DashBoard.pbix      # Power BI file
└── images/
    └── dashboard.png             # Dashboard preview
```

---

## 🔄 Pipeline Flow

### 1. Ingestion — CSV → PostgreSQL Raw
`python/ingestion/load_raw_data.py` reads the Superstore CSV with `pandas`, normalizes all column names (lowercase, underscores), and loads the full dataset into `raw.superstore_raw` via SQLAlchemy:

```python
df = pd.read_csv("data/raw/train.csv", encoding="latin1")
df.columns = [col.strip().lower().replace(" ", "_") for col in df.columns]
df.to_sql("superstore_raw", con=engine, schema="raw", if_exists="replace")
```

### 2. Staging — SQL Transformations
`sql/staging/stg_superstore.sql` creates a clean staging table with:
- Date conversion: `TO_DATE(order_date, 'DD/MM/YYYY')`
- Type casting: `sales::numeric(12,2)`
- Structured column selection for downstream modeling

```sql
CREATE TABLE staging.stg_superstore AS
SELECT
    order_id,
    TO_DATE(order_date, 'DD/MM/YYYY') AS order_date,
    TO_DATE(ship_date, 'DD/MM/YYYY')  AS ship_date,
    customer_id, customer_name, segment,
    region, category, sub_category, product_name,
    sales::numeric(12,2) AS sales
FROM raw.superstore_raw;
```

### 3. Star Schema
Dimensional model built from the staging layer:

| Table | Type | Key Fields |
|---|---|---|
| `fact_sales` | Fact | order_id, sales, customer_key, product_key, date_key |
| `dim_customers` | Dimension | customer_id, name, segment, region |
| `dim_products` | Dimension | product_id, name, category, sub_category |
| `dim_date` | Dimension | date_key, year, month, quarter |

### 4. Power BI Dashboard
Connected to PostgreSQL star schema. KPIs and visuals include:
- **Revenue** and **Profit Margin** by period
- **Sales trends** over time (2015–2018)
- **Top products** and **top regions** by revenue
- **Customer segmentation** (Consumer, Corporate, Home Office)

---

## 🚀 How to Run

### Prerequisites
- Python 3.10+
- PostgreSQL running locally on port 5432
- Database `bi_sales_project` created with schemas `raw` and `staging`

### Setup

```bash
# Clone the repository
git clone https://github.com/sbarrosh/end-to-end-bi-sales-analytics.git
cd end-to-end-bi-sales-analytics

# Install dependencies
pip install pandas sqlalchemy psycopg2-binary

# Configure database credentials
# Edit python/config/db_config.py with your PostgreSQL credentials

# Run ingestion
python main.py

# Run staging SQL
# Execute sql/staging/stg_superstore.sql in your PostgreSQL client
```

### Open Dashboard
Open `dashboards/Sales DashBoard.pbix` in Power BI Desktop and update the data source connection to your local PostgreSQL.

---

## 📊 Dataset

**Superstore Sales** — a widely used retail analytics dataset containing US sales transactions from 2015 to 2018:

| Field | Description |
|---|---|
| `order_id` | Unique order identifier |
| `order_date` | Date of purchase |
| `customer_name` | Customer full name |
| `segment` | Consumer / Corporate / Home Office |
| `region` | US region (East, West, Central, South) |
| `category` | Furniture / Office Supplies / Technology |
| `sales` | Revenue per line item (USD) |

---

## 💡 Key Learnings

- Designing a **two-layer PostgreSQL architecture** (raw + staging schemas) for progressive data refinement
- Loading CSVs into a database using **SQLAlchemy + Pandas `to_sql()`**
- Writing **SQL type casting and date transformations** for data cleaning
- Building a **star schema** (fact + dimensions) for analytical queries
- Connecting **Power BI to PostgreSQL** and building business dashboards

---

## 🔜 Next Steps

- [ ] Build the full star schema DDL in SQL
- [ ] Add a Gold layer with pre-aggregated metrics
- [ ] Schedule ingestion with Apache Airflow
- [ ] Add data quality checks (nulls, duplicates, out-of-range sales)
- [ ] Publish dashboard to Power BI Service

---

## Dashboard Preview

![Dashboard](images/dashboard.png)