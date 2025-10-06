## Exercise Documentation

## Approach Overview

I approached this exercise in the following **phases**:

1. **Understanding the problem statement** – reviewing the objectives, identifying key considerations and assumptions, and planning the overall approach.  
2. **Designing the high-level architecture** – outlining the medallion layers and overall data flow.  
3. **Defining the data layouts** – structuring how raw, silver, and gold data would be organized.  
4. **Planning control tables** – establishing configuration tables to drive the orchestration logic.  
5. **Building the orchestration notebook** – creating parameterized notebooks to manage end-to-end execution.  
6. **Setting up pipelines and jobs** – orchestrating tasks, enabling batch and parallel execution.  
7. **Performing data validation and testing** – ensuring data accuracy, consistency, and completeness.  
8. **Developing the star schema and business views** – implementing curated models, solving the problem statement, evaluating results, and documenting the complete process.
9. **Solution** – utilized the business view (`vw_investor_relation`) created at step-8 to address the four problem statements listed below.  
10. **Documentation** – documented the entire process for easy sharing and better understanding.  


Used:

**Databricks for ETL and Orchestration.**

**Github for code-repository and documentaiton.**

## Probelm Statments

1. Summarize overall performance of fund 'Fund Two' for Investor 7002.
2. Compare overall return of Investor (7002) with others.
3. Show trend of MoM sinec initial investments.
4. Validate data and state any adjustments.

## Solution

**Solution notebook link** - https://github.com/anoopdk/medallion_project/blob/54727627337bfa87da9cd1a663d710967cc692d8/scripts/exercise/exercise_QnA.ipynb

**Databricks work link** - https://github.com/anoopdk/medallion_project/blob/main/docs/databricks%20work%20documentation.md

## Data Architecture
![Data Architecture Diagram](https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Data%20Architecture%20Diagram.png)

This project implements a Medallion Architecture data pipeline that sources data from CSV files and processes it through structured layers to deliver high-quality, business-ready datasets.

- **Bronze Layer (Raw):** Ingests CSV data with minimal transformation.  
- **Silver Layer (Enriched):** Cleanses, validates, standardizes data, and preserves history.  
- **Gold Layer (Curated):** Applies business rules, performs aggregations, and builds star schema models.  
- **Consumption:** Enables BI reporting, machine learning, and ad-hoc analysis.  
- Ensures **data quality**, **scalability**, and **clear separation of processing layers**.  


## Data Layers Overview
![Data Layout](https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Data%20Laytout%20Diagram.JPG)

The data pipeline follows a structured **Medallion Architecture** with three distinct layers, **Bronze**, **Silver**, and **Gold**, each serving a specific purpose in the data lifecycle from ingestion to business consumption.

- **Bronze Layer (Raw):**  
  - **Definition:** Stores raw, unprocessed data exactly as received from source systems.  
  - **Objective:** Ensures traceability and debugging with a complete data record.  
  - **Data Transformation:** None (as-is).  
  - **Data Modelling:** None (as-is).  
  - **Target Audience:** Data Engineers.  

- **Silver Layer (Enriched):**  
  - **Definition:** Contains clean, standardized, and enriched data ready for analysis.  
  - **Objective:** Provides structured, analysis-ready datasets.  
  - **Data Transformation:**  
    - Data Cleaning  
    - Data Standardization  
    - Data Enrichment  
    - Derived Columns  
  - **Data Modelling:** None (as-is).  
  - **Target Audience:** Data Analysts, Data Scientists.  

- **Gold Layer (Curated):**  
  - **Definition:** Holds curated, business-ready, and aggregated data for reporting and analytics.  
  - **Objective:** Deliver integrated datasets for business intelligence and decision-making.  
  - **Data Transformation:**  
    - Data Integration  
    - Data Aggregation  
    - Application of Business Logic  
  - **Data Modelling:** Star Schema and Aggregations.  
  - **Target Audience:** Business Users, Data Analysts, Data Scientists.  


## Metadata Driven Orchestration
![Metadata-driven contrrol tables](https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Metadata%20Control%20Flow%20Diagram.png)

This architecture implements a **metadata-driven approach** to automate and standardize the data pipeline execution across Bronze, Silver, and Gold layers.  

Control tables define how data should be extracted, validated, transformed, and loaded at each stage, ensuring flexibility, reusability, governance, and this approach promotes scalability.

- **Bronze Source Files Control Table:**  
  - **Source_File_Details:** Stores metadata such as file paths, formats, headers, load types, and storage locations.  
  - **Source_File_Specifications:** Captures column-level metadata including data types, field order, and descriptions.  
  - **Purpose:** Enables dynamic ingestion of CSV files into the Bronze layer without hardcoding and executes via table configurations.  

- **Enrich Data Quality Control Table (Silver):**  
  - **Enrich_Table_Constraints:** Defines data-quality rules, column-level constraints, and renaming logic.  
  - **Purpose:** Ensures clean, standardized, and constraint validated data in the Silver layer.  

- **Gold Tables Mapping Control Table:**  
  - **Curated_Table_Mapping:** Specifies how Silver tables map into Gold tables, including join logic and transformation rules.  
  - **Purpose:** Automates the creation of curated, business-ready datasets following star schema modeling principles.  

This metadata-driven design ensures **scalable orchestration**, **parallel execution**, and **easy maintenance** across the Medallion pipeline.



## Notebook & Orchestration Setup
![Pipeline Execution](https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Pipeline%20Orchestration%20Flow%20Diagram.png)

The pipeline implements a **parameterized, metadata-driven orchestration** that automates data processing across all Medallion layers, Bronze, Silver, and Gold.  
It is designed for **scalability**, **reusability**, and **parallel execution**.

- **Raw Data Load (Bronze):**  
  Ingests source CSV files as-is, maintaining complete data traceability.

- **Enrichment Loads (Silver):**  
  - Processes run in parallel for each entity, *Investor*, *Investment*, and *Transaction*.  
  - Applies cleaning, enrichment, and standardization rules using metadata control tables.  
  - Ensures data consistency and quality before moving to the curated layer.

- **Gold Load (Star Schema):**  
  - Integrates enriched data to build **fact** and **dimension** tables.  
  - Produces a **business-ready star schema model** optimized for analytics and reporting.  
  - Loads are fully automated based on mappings defined in metadata control tables.

Databricks Job: https://dbc-0f540a54-8135.cloud.databricks.com/jobs/441984073143903?o=1782094926787982 

This end-to-end workflow ensures a seamless transition of data through the Medallion layers, enabling **automated, parallel, and reliable** data pipeline execution.



## Star Schema Model & Business View
![Star Schema](https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Start%20Schema%20Model.drawio.png)

The **Gold Layer** of the Medallion Architecture is designed using a **Star Schema** model to support efficient analytics, reporting, and business insights.  
This schema integrates cleansed and enriched data from the Silver layer into a unified, business-focused structure.

- **Fact Table – `fact_transaction`:**  
  - Contains quantitative, transactional data such as investment amounts, transaction dates, and fund details.  
  - Acts as the central table linked to multiple dimensions.  
  - Enables performance tracking and trend analysis across funds and investors.  

- **Dimension Tables:**  
  - **`dim_investor`** – Holds investor details such as investor type and identifiers.  
  - **`dim_investment`** – Contains investment-specific metadata like entry and exit dates.   

- **🔗 Entity Relationships:**  
  - Each dimension connects to the fact table via surrogate keys.  
  - The model simplifies querying by separating business entities (dimensions) from measurable events (facts).  


The Star Schema ensures that curated data is **optimized for analytics**, **easy to query**, and **aligned with business use cases**.


## Business View – `vw_investor_relation`

To simplify analytics and provide a unified perspective, a **business view** (`vw_investor_relation`) has been designed on top of the **Gold Layer** star schema.  
This view brings together key information from the **fact** and **dimension** tables to deliver a holistic understanding of investor performance.

**Reference - scripts/business views/bus_view_investor_relation.ipynb**


## **REFERENCES**

### Main Links:

Solution notebook - https://github.com/anoopdk/medallion_project/blob/54727627337bfa87da9cd1a663d710967cc692d8/scripts/exercise/exercise_QnA.ipynb

Business view - https://github.com/anoopdk/medallion_project/blob/54727627337bfa87da9cd1a663d710967cc692d8/scripts/business%20views/bus_view_investor_relation.ipynb

Orcheatration notebooks - https://github.com/anoopdk/medallion_project/tree/54727627337bfa87da9cd1a663d710967cc692d8/scripts/orchestration

Jobs - https://github.com/anoopdk/medallion_project/blob/54727627337bfa87da9cd1a663d710967cc692d8/scripts/jobs/job_investor_relation_raw_to_curated.ipynb


### Diagram source files:

Architecture - https://github.com/anoopdk/medallion_project/blob/84b83daa083a844bc8113a6958571646b9619c73/docs/Data%20Architecture%20Diagram.drawio

Orchestration & Schema - https://github.com/anoopdk/medallion_project/blob/c95163ef55f38ff1d4dde7cda813c3e1e78b3a2b/docs/Data%20Layers%20and%20Orchestration%20Diagram.drawio 


### Miscellaneous 

Contrrol tables DDL script - https://github.com/anoopdk/medallion_project/blob/54727627337bfa87da9cd1a663d710967cc692d8/scripts/sql%20DDL%20scripts/control_tables_ddl_insert.ipynb

Utility notebook - https://github.com/anoopdk/medallion_project/tree/54727627337bfa87da9cd1a663d710967cc692d8/scripts/utilities


