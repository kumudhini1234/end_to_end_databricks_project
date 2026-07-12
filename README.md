# FMCG End-to-End Data Engineering Project 🚀

An end-to-end data engineering pipeline built on **Databricks Free Edition**, simulating a real-world FMCG (Fast-Moving Consumer Goods) sales analytics use case. The project follows the **Medallion Architecture** (Bronze → Silver → Gold) to transform raw, messy transactional data into clean, business-ready insights — visualized through an interactive dashboard.

---

## 📌 Project Overview

This project simulates the data pipeline of an FMCG company that needs visibility into monthly sales performance across products, customers, and regions to support decision-making.

**Goals:**
- Ingest raw sales/order data reliably
- Clean and standardize data for consistency and trust
- Aggregate data into business-friendly metrics
- Deliver insights through a dashboard

---

## 🏗️ Architecture

```
Raw Data Source
      │
      ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   BRONZE    │ --> │   SILVER    │ --> │    GOLD     │ --> Dashboard
│ (raw data)  │     │ (cleaned)   │     │(aggregated) │
└─────────────┘     └─────────────┘     └─────────────┘
```

| Layer | Purpose |
|---|---|
| **Bronze** | Raw data ingested as-is from the source, preserving original structure |
| **Silver** | Cleaned and transformed data — deduplication, null handling, date/schema fixes |
| **Gold** | Aggregated, business-ready tables (monthly sales, top products, top customers) |

---

## 🛠️ Tech Stack

- **Platform:** Databricks Free Edition
- **Processing:** PySpark, Spark SQL
- **Storage Format:** Delta Lake
- **Orchestration:** Databricks Notebooks / [Workflows if used]
- **Visualization:** [Databricks SQL Dashboard / Power BI / Tableau — update accordingly]

---

## 📂 Repository Structure

```
├── consolidated_pipeline/        # All notebooks — Bronze, Silver, Gold + dashboard queries
├── 1_parent_company/             # Raw data (CSV)
├── customers/                    # Raw data (CSV)
├── gross_price/                  # Raw data (CSV)
├── products/                     # Raw data (CSV)
├── incremental_load/
│   └── orders/                   # Incremental order data (CSV)
└── README.md
```

- **`consolidated_pipeline/`** contains all the notebooks — Bronze ingestion, Silver transformations, Gold aggregations, and dashboard/reporting queries.
- The remaining folders (`1_parent_company`, `customers`, `gross_price`, `products`, `incremental_load/orders`) hold the raw source CSV files used as inputs to the pipeline, organized by data domain.

> Update this section further if you rename folders or add new ones (e.g., images, docs).

---

## ⚙️ Pipeline Details

All pipeline logic lives in the [`consolidated_pipeline/`](./consolidated_pipeline) folder, structured as a set of notebooks covering the Bronze → Silver → Gold flow plus dashboard queries.

### 1. Bronze Layer — Raw Ingestion
- Reads raw source CSVs (`1_parent_company`, `customers`, `gross_price`, `products`, `incremental_load/orders`) into Delta tables with minimal transformation.
- `incremental_load/orders` is handled separately to simulate incremental (rather than full/batch) ingestion of new order data.
- Preserves original data for traceability/auditability.

### 2. Silver Layer — Cleaning & Transformation
- Handles null values and duplicate records
- Standardizes date formats (e.g., stripping weekday prefixes like `"Mon, 2024-03-11"` → `"2024-03-11"`)
- Fixes schema inconsistencies across source tables

### 3. Gold Layer — Aggregation
- Truncates dates to monthly grain using `F.trunc("date", "MM")`
- Aggregates `sold_quantity` by `month_start`, `product_code`, and `customer_code`
- Joins in dimension data (parent company, customers, products, gross price) to produce clean, query-ready tables for reporting

### 4. Dashboard
- Top products by revenue
- Revenue share by Channel
- Monthly Revenue Trend

📸 *Dashboard Preview:*

<img width="2214" height="1629" alt="dashboard" src="https://github.com/user-attachments/assets/80da7f36-ce49-4ab2-929f-0f0eba8635f6" />


---

## 🚧 Challenges & Learnings

- Debugging `NameError` issues caused by Databricks Free Edition's serverless compute resetting session state between runs
- Writing robust regex-based date cleaning to handle inconsistent date string formats
- Understanding the tradeoffs between `F.trunc()` and `F.date_trunc()` for date-level aggregations
- Structuring notebooks to follow Medallion Architecture best practices

---

## ▶️ How to Run

1. Clone this repo
2. Import the notebooks from `consolidated_pipeline/` into your Databricks workspace
3. Upload the CSV files from `1_parent_company`, `customers`, `gross_price`, `products`, and `incremental_load/orders` to your Databricks workspace/volume (or point the pipeline at wherever you store them)
4. Update the `catalog`, `schema`, and `data_source` variables in the config/setup cell
5. Run the notebooks in order (Bronze → Silver → Gold → dashboard queries)

---

## 🙌 Acknowledgements

Thanks to [CodeBasics](https://www.youtube.com/@codebasics) for the excellent project walkthrough that guided the structure of this pipeline.

---

## 📬 Contact

Kumudhini Reddicherla — https://www.linkedin.com/in/kumudhini-reddicherla-4ba5b4261/ • kumudhini.reddicherla@gmail.com 
