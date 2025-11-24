# Retail Orders Data Engineering Pipeline

This repository contains a complete, production-style **data engineering pipeline** built around the Retail Orders dataset.
It includes raw data ingestion, profiling, data quality checks, cleansing, dimensional modeling, data marts generation, and analytics visualizations.

The structure follows modern data engineering best practices with clear separation between:

* **Raw Data**
* **Staging Layer**
* **Data Warehouse Dimensions & Facts**
* **Analytical Data Marts**
* **DQ Reporting**
* **Visual Analytics**
* **Documentation (HLD, ERD, DDL)**

---

# 📂 Repository Structure

```
├── data_raw/                               # Raw monthly CSV snapshots
│
├── notebook/
│   └── data_analysis.ipynb.ipynb           # Full pipeline notebook: ingestion → DQ → staging → DWH → marts → visuals
│ 
├── diagrams/
│   ├── Task_2_HLD.pptx                     # High-level architecture (pipeline design)
│   ├── Task_3_ERD.png                      # Entity-Relationship Diagram
│   └── Task_4_DDL.txt                      # Full SQL DDL for schemas & tables
│
├── outputs/
│   ├── dq_exceptions.csv                   # Data quality inconsistencies detected
│   ├── Task_5_Inconsistencies_Analysis.xlsx # DQ summary + examples + recommended actions
│   │
│   ├── staging/                            # Standardized, typed staging outputs
│   │   └── orders_stg.csv
│   │
│   └── marts/                              # Analytical marts exported as CSV
│       ├── product_monthly_sales.csv
│       ├── state_monthly_sales.csv
│       ├── customer_segment_monthly_sales.csv
│       └── product_state_delivery.csv
│
└── README.md
```

---

# 🔧 Pipeline Overview

All pipeline steps are implemented in the notebook:

`notebook/data_analysis.ipynb`

The notebook performs:
### **1. Setup**

### **2. Data Extraction**

* Loads all raw monthly CSV snapshots
* Automatic detection of file month from filename
* Handles encoding, delimiters, quoting, parsing issues

### **3. Data Profiling**

* Column completeness & missingness
* Numeric and categorical distributions
* Date range profiling
* Outlier detection

### **4. Data Quality Detection**

A full inconsistency framework identifies:

* Duplicate rows across file versions
* Sales/Quantity = 0 but Profit ≠ 0
* Product attribute conflicts (cross-month and cross-file)
* Negative quantities
* Invalid or missing dates and fields

All issues exported to:

* `outputs/dq_exceptions.csv`
* `outputs/Task_5_Inconsistencies_Analysis.xlsx`

### **5. Staging Layer**

* Typed conversion (dates, numerics)
* Standardized structure
* Month key & source metadata
* Output stored in: `outputs/staging/orders_stg.csv`

### **6. Master Data (Golden Record Logic)**

For stable attributes, the latest seen version is selected:

* Product Category, Subcategory, Name
* Customer Name, Segment

Ensures consistent dimension attributes across marts.

### **7. Data Marts**

Four analytical marts generated:

| Mart File                            | Description                                                |
| ------------------------------------ | ---------------------------------------------------------- |
| `product_monthly_sales.csv`          | Sales + quantity + avg selling price per product per month |
| `state_monthly_sales.csv`            | Sales & order counts per state per month                   |
| `customer_segment_monthly_sales.csv` | Customer-level segment performance                         |
| `product_state_delivery.csv`         | Avg delivery days per product/state per month              |

All stored in `outputs/marts/`.

### **8. Visual Analytics**

The notebook includes polished charts:

* Total sales trend
* Top performing states
* Delivery days distribution

These visuals help interpret the marts and validate data quality.

---

# 🗄️ Documentation

All architectural documentation is included:

| File                       | Purpose                                                                 |
| -------------------------- | ----------------------------------------------------------------------- |
| `diagrams/Task_2_HLD.pptx` | High-level architecture (ingestion → staging → DWH → marts → reporting) |
| `diagrams/Task_3_ERD.png`  | Entity-Relationship Diagram (raw, staging, DWH, marts)                  |
| `diagrams/Task_4_DDL.txt`  | Full SQL DDL (schemas & tables)                                         |

---

# 🚀 Running the Pipeline

### **1. Clone the repository**

```bash
git clone <your_repo_url>
cd <repo_folder>
```

### **2. Install dependencies**

```bash
pip install -r requirements.txt
```

### **3. Run the notebook**

Open:

```
notebook/data_analysis.ipynb
```
---

# 📝 Notes

* All raw inputs remain unmodified in `data_raw/`
---