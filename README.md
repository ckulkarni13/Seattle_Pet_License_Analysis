# Seattle Pet License: Cross-Database ETL Pipeline

A professional data engineering project implementing a robust ETL (Extract, Transform, Load) workflow to process municipal pet license data. This project showcases the use of **Talend Open Studio** to synchronize large-scale records across heterogeneous database environments while ensuring strict data quality.

## 📌 Project Overview
The Seattle Pet License dataset contains high-velocity registration records that require significant cleansing before they are "analytics-ready." This pipeline automates the extraction of 42,000+ raw records, applies complex schema mappings, and performs parallel loading into multiple staging environments for downstream business intelligence.

## 🛠 Tech Stack
* **ETL Orchestration:** Talend Open Studio
* **Target Databases:** MySQL, Microsoft SQL Server (SSMS)
* **Data Handling:** T-SQL, MySQL Workbench
* **Monitoring:** Talend Execution Console

## Database Schema
The staging tables (`stg_spl_output` in MySQL and `stg_spl_demo` in SQL Server) utilize the following structure to ensure data integrity during the initial load:

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `License_Issue_Date` | `datetime` | The date the pet license was issued. |
| `License_Number` | `varchar(10)` | Unique identifier for the license. |
| `Animal_s_Name` | `varchar(80)` | The name of the registered animal. |
| `Species` | `varchar(20)` | The category of pet (e.g., Cat, Dog). |
| `Primary_Breed` | `varchar(80)` | The primary breed classification. |
| `Secondary_Breed` | `varchar(80)` | The secondary breed classification, if applicable. |
| `ZIP_Code` | `varchar(20)` | Postal code for the registration. |
| `DI_Create_Date` | `datetime` | Metadata: Timestamp of record insertion. |
| `DI_JobPID` | `varchar(20)` | Metadata: Identifier for the specific ETL process run. |

---

## 🏗 Pipeline Architecture & Workflow
The system is designed for high-throughput and parallel data delivery:

1.  **Extraction:** Connects to the raw Seattle municipal data source to pull 42k+ registration entries.
2.  **Transformation (tMap_1):** * Implements strict schema mapping and data type casting.
    * Metadata Enrichment: Automatically injects `DI_Create_Date` and `DI_JobPID` for auditability and lineage tracking.
3.  **Parallel Loading:** Simultaneously populates two distinct staging environments:
    * **MySQL Staging (`stg_spl_output`)**
    * **MS SQL Server Staging (`stg_spl_demo`)**


## 🛡️ Data Quality & Cleansing Logic
A critical component of this project was identifying and resolving data integrity bottlenecks during execution:

* **Type Mismatch Resolution:** Identified a failure in the `Zip_Code` field where integer constraints rejected alphanumeric postal codes (e.g., `"V5J1P8"`).
* **Automated Correction:** Reconfigured the `tMap` component to handle `Zip_Code` as a `String` (Varchar), enabling a 100% successful ingestion of the final record.
* **Result:** Increased data throughput from 42,525 to 42,526 rows, ensuring zero data loss.

## 📊 Performance Statistics
* **Total Records Processed:** 42,526
* **Job Success Rate:** 100%
* **Throughput:** ~2,000 rows per second across dual targets.

## 📂 Repository Contents
* `process/`: Core Talend job definitions and XML configurations.
* `metadata/`: Schema definitions and connection parameters.
* `Seattle_Pet_License_Talend_Report.pdf`: Comprehensive technical documentation and execution logs.
* `talend.project`: The primary project file for importing into Talend Open Studio.


### Images from Project -->
![SQL Data Loading](https://github.com/ckulkarni13/Designing_Advanced_Data_Architectures_for_Business_Intelligence/blob/main/Seattle%20Pet%20License/mysql_loaded_Data.png?raw=true)

---
*Developed by [Chinmay Kulkarni](https://github.com/ckulkarni13)*
