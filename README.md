# Seattle Pet License ETL Project

This repository contains the ETL (Extract, Transform, Load) workflow for processing Seattle pet license data into a staging environment. The project demonstrates cross-database integration and data cleansing techniques using industry-standard tools.

---

## Technical Stack
* **ETL Tool:** Talend Open Studio
* **Databases:** MySQL and Microsoft SQL Server (SSMS)
* **Data Sources:** Seattle Pet License raw data

---

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

## ETL Workflow & Architecture
The Talend job `Job_Seattle_Pet_License_Job_0.1` is designed for parallel loading and error handling:

1.  **Extraction:** `SPL_read_Data` retrieves raw records from the source.
2.  **Transformation:** `tMap_1` handles schema mapping and data type conversions.
3.  **Loading:** Data is simultaneously pushed to:
    * **target_mssqlServer:** For Microsoft SQL Server environments.
    * **target_mysql:** For MySQL environments.

---

## Data Quality & Error Handling
During execution, a data type mismatch was identified in the `Zip_Code` field:
* **Constraint:** The `Zip_Code` field was initially set as an `Integer`, causing failures for alphanumeric values like `"V5J1P8"`.
* **Correction:** The schema was updated to a `String` (varchar) within the `tMap` component to support diverse postal code formats.
* **Impact:** Resolving this ensured the successful ingestion of the final record, increasing the load from 42,525 to 42,526 rows.

---

## Final Execution Statistics
The workflow achieved 100% data throughput:
* **Total Rows Processed:** 42,526.
* **Workflow Status:** Query executed successfully across both target environments.
