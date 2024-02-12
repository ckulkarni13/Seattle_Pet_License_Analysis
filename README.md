# Seattle Pet License ETL Project

This repository contains the ETL (Extract, Transform, Load) workflow for processing Seattle pet license data into a staging environment. [cite_start]The project demonstrates cross-database integration and data cleansing techniques using industry-standard tools[cite: 2, 465].

---

## Technical Stack
* [cite_start]**ETL Tool:** Talend Open Studio[cite: 2, 465].
* [cite_start]**Databases:** MySQL and Microsoft SQL Server (SSMS)[cite: 177, 433].
* [cite_start]**Data Sources:** Seattle Pet License raw data[cite: 2].

---

## Database Schema
[cite_start]The staging tables (`stg_spl_output` in MySQL and `stg_spl_demo` in SQL Server) utilize the following structure to ensure data integrity during the initial load[cite: 10, 216]:

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `License_Issue_Date` | `datetime` | [cite_start]The date the pet license was issued[cite: 17, 134]. |
| `License_Number` | `varchar(10)` | [cite_start]Unique identifier for the license[cite: 17, 137]. |
| `Animal_s_Name` | `varchar(80)` | [cite_start]The name of the registered animal[cite: 20, 143]. |
| `Species` | `varchar(20)` | [cite_start]The category of pet (e.g., Cat, Dog)[cite: 22, 148]. |
| `Primary_Breed` | `varchar(80)` | [cite_start]The primary breed classification[cite: 23, 152]. |
| `Secondary_Breed` | `varchar(80)` | [cite_start]The secondary breed classification, if applicable[cite: 25, 158]. |
| `ZIP_Code` | `varchar(20)` | [cite_start]Postal code for the registration[cite: 26, 164]. |
| `DI_Create_Date` | `datetime` | [cite_start]Metadata: Timestamp of record insertion[cite: 27, 168]. |
| `DI_JobPID` | `varchar(20)` | [cite_start]Metadata: Identifier for the specific ETL process run[cite: 30, 171]. |

---

## ETL Workflow & Architecture
[cite_start]The Talend job `Job_Seattle_Pet_License_Job_0.1` is designed for parallel loading and error handling[cite: 468]:

1.  [cite_start]**Extraction:** `SPL_read_Data` retrieves raw records from the source[cite: 476].
2.  [cite_start]**Transformation:** `tMap_1` handles schema mapping and data type conversions[cite: 477].
3.  **Loading:** Data is simultaneously pushed to:
    * [cite_start]**target_mssqlServer:** For Microsoft SQL Server environments[cite: 474].
    * [cite_start]**target_mysql:** For MySQL environments[cite: 482].

---

## Data Quality & Error Handling
[cite_start]During execution, a data type mismatch was identified in the `Zip_Code` field[cite: 587]:
* [cite_start]**Constraint:** The `Zip_Code` field was initially set as an `Integer`, causing failures for alphanumeric values like `"V5J1P8"`[cite: 589, 590].
* [cite_start]**Correction:** The schema was updated to a `String` (varchar) within the `tMap` component to support diverse postal code formats[cite: 591].
* [cite_start]**Impact:** Resolving this ensured the successful ingestion of the final record, increasing the load from 42,525 to 42,526 rows[cite: 592].

---

## Final Execution Statistics
[cite_start]The workflow achieved 100% data throughput[cite: 611]:
* [cite_start]**Total Rows Processed:** 42,526[cite: 526, 583].
* [cite_start]**Workflow Status:** Query executed successfully across both target environments[cite: 431, 585].

---

Would you like me to help you draft the **DDL SQL scripts** for these tables so you can include them in a `/scripts` folder in your repository?
