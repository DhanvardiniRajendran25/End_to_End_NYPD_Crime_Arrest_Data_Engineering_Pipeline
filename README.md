# End-to-End NYPD Crime Arrest Data Engineering Pipeline

## 📌 Overview
This project focuses on building a **scalable end-to-end data pipeline** for NYPD Arrest and Crime Data.  
Data is sourced from the [NYC Open Data Portal](https://data.cityofnewyork.us/Public-Safety/NYPD-Arrest-Data-Year-to-Date-/uip8-fykc/data_preview), which contains records since 2018.  

The pipeline ingests, cleans, transforms, and loads the data into **Snowflake**, enabling advanced analytics and rich visualizations to support crime pattern analysis, demographic insights, and precinct-level reporting.

---

## 🧰 Technology Stack

**Cloud & Storage**
- **Azure Data Factory (ADF)** – pipeline orchestration (parameterized, incremental loads)
- **Azure Data Lake / Blob** – landing & staging
- **Snowflake** – cloud data warehouse (fact/dim tables, analytics)

**Data Modeling & SQL**
- **ER/Studio** – dimensional modeling (star schema, SCD)
- **DBeaver** – SQL development, Snowflake connectivity & validation
- **SQL / SnowSQL** – DDL/DML, MERGE operations (SCD Type 1/2)

**Transformation & Quality**
- **Alteryx** – deterministic cleaning, standardization, and enrichment workflows
- **Python (ydata-profiling / pandas)** – data profiling & data quality assessment

**Visualization & Collaboration**
- **Power BI/Tableau** – dashboards & insights
- **Git & GitHub** – version control and project management

---

## 🎯 Business Goal

Provide a **single source of truth** for crime & arrest analytics across **time, location, and demographics**, enabling operational reporting and trend analysis at **borough, precinct, and offense** levels.

---

## 📦 Data & Grain

- **Fact**: One row per **arrest event** (grain = `ARREST_KEY`)
- **Dimensions**:
  - `DIM_DATE` (calendar attributes)
  - `DIM_BOROUGH`
  - `DIM_PRECINCT`
  - `DIM_LOCATION` *(SCD2 when needed)*
  - `DIM_OFFENSE` *(law category, offense level)*
  - `DIM_LAW`
  - `DIM_PERPETRATOR` *(Age group, Race, Sex; SCD2 if tracked historically)*

---

## 🧭 End-to-End Workflow

1. **Ingest & Land**
   - Download NYPD Arrest data (YTD) from NYC Open Data.
   - Land in Azure Storage as CSV/TSV.

2. **Profile**
   - Run **Python ydata-profiling** to detect missing values, type issues, ranges, and outliers.
   - Produce HTML profiling report for documentation.

3. **Model**
   - Use **ER/Studio** to design a **Star Schema** optimized for analytic queries.
   - Define keys, PK/FK relationships, and SCD strategy.

4. **Clean & Transform**
   - Build **Alteryx** workflows to:
     - Standardize borough and precinct codes
     - Normalize age, race, sex
     - Parse & conform dates to DIM_DATE
     - Map offense & law attributes

5. **Load to Snowflake**
   - Create **staging** and **dim/fact** tables (DBeaver/SnowSQL).
   - Use **ADF** pipelines for **incremental loads** (watermark pattern), with parameterized datasets and logging.
   - Apply **MERGE** for SCD Type 1/2 where appropriate.

6. **Serve & Visualize**
   - Expose curated **gold** tables in Snowflake.
   - Build **Power BI/Tableau** dashboards (time trends, borough/precinct hotspots, offense mix, demographic splits).

---

## 🧩 Project Workflow

<img width="468" height="860" alt="image" src="https://github.com/user-attachments/assets/93465606-35ac-4e1b-8732-1a027eb644df" />


### 1. Data Profiling
- Used Python (yDataProfiling) to identify missing values, inconsistencies, and datatype errors.
- Found issues like missing **LAW_CAT_CD**, inconsistent age group formats, and geolocation gaps

### 2. Dimensional Modeling
- Designed a **Star Schema** in ER Studio:
  - Fact: `FACT_ARRESTS` (one record per arrest event, grain = arrest key)
  - Dimensions:
    - `DIM_PRECINCT`
    - `DIM_BOROUGH`
    - `DIM_LOCATION` (SCD Type 2)
    - `DIM_OFFENSE`
    - `DIM_LAW`
    - `DIM_PERPETRATOR` (SCD Type 2)

### 3. Data Cleaning & Transformation
- Built Alteryx workflows to:
  - Standardize borough codes
  - Handle missing values
  - Normalize age, race, and gender attributes
  - Convert dates into usable formats (day/week/month/year)

### 4. Pipeline Orchestration
- Implemented **Azure Data Factory** pipeline:
  - Incremental loads into Snowflake
  - Parameterized pipelines for reusability
  - Error handling and monitoring

### 5. Cloud Data Warehouse
- Snowflake stores:
  - Arrest fact table
  - Dimension tables
  - Supports analytical queries on trends, geography, and demographics

### 6. Visualization

<img width="1203" height="649" alt="image" src="https://github.com/user-attachments/assets/5ae4b874-cc3d-4f8f-84b8-69f516c26544" />

- Built **interactive dashboards** (Power BI/Tableau) to analyze:
  - Time-based patterns (daily, weekly, monthly, yearly trends)
  - Crime types and top offenses  
  - Borough and precinct-level hotspots  
  - Demographic distribution by age, race, and gender

---

## 📊 Key Insights
- Peak month: **August 2024 (22,957 arrests)**
- Top Crimes: **Assault 3 & Related, Petit Larceny, Felony Assault, Dangerous Drugs**
- Borough with highest arrests: **Brooklyn (72,325 arrests)**
- Age group with most arrests: **25–44 years (152,034 arrests)**
- Race distribution: **Black (122,049), White Hispanic (69,131), Black Hispanic (26,549)** 
- High-crime precinct: **Precinct 14 (9,887 arrests)**

---

## 🔒 Data Quality & Governance

* **Profiling**: Automated HTML report via **ydata-profiling**
* **Standardization**: Alteryx rules for codes, date parsing, demographic buckets
* **SCD**: `DIM_LOCATION` / `DIM_PERPETRATOR` can be modeled as SCD2 (validity date ranges)
* **Validation**: DBeaver SQL checks for referential integrity, nulls, and conformance

---

## 🧪 KPIs & Analytics

* Arrests by **month/weekday/hour**
* Heatmaps by **borough / precinct**
* Distribution by **offense, law category**
* Demographics by **age group, race, sex**
* Rolling trends and seasonality indicators

---

## 📜 Notes & Attributions

* The NYPD dataset is **continuously updated**; metrics will shift by date.
* Please refer to NYC Open Data licensing for permitted reuse.

---

## 👩‍💻 Author

**Dhanvardini Rajendran**
GitHub: [@DhanvardiniRajendran25](https://github.com/DhanvardiniRajendran25)

---
