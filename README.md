# 🏢 Employee ETL Pipeline — SSIS Project

A full ETL pipeline built with SQL Server Integration Services (SSIS) for Employee data, implementing a 3-layer architecture: ODS → STG → DWH.

## 🏗️ Architecture


 ## 📁 Data Sources & Transformations

### Source 1 — Excel (.xlsx)
- **Gender:** `gender == "M" ? "Male" : gender == "F" ? "Female" : "Unknown"`
- **Date (format: MM/DD/YYYY → YYYY-MM-DD):**
  `SUBSTRING(age,FINDSTRING(age,"/",2)+1,4) + "-" + RIGHT("0" + SUBSTRING(age,1,FINDSTRING(age,"/",1)-1),2) + "-" + RIGHT("0" + SUBSTRING(age,FINDSTRING(age,"/",1)+1,FINDSTRING(age,"/",2)-FINDSTRING(age,"/",1)-1),2)`

### Source 2 — CSV (.csv)
- **Gender:** `gender == "M" ? "Male" : gender == "F" ? "Female" : gender`
- **Date (format: DD.MM.YYYY → YYYY-MM-DD):**
  `SUBSTRING(age,FINDSTRING(age,".",2)+1,4) + "-" + RIGHT("0" + SUBSTRING(age,FINDSTRING(age,".",1)+1,FINDSTRING(age,".",2)-FINDSTRING(age,".",1)-1),2) + "-" + RIGHT("0" + SUBSTRING(age,1,FINDSTRING(age,".",1)-1),2)`

### Source 3 — XML (.xml)
- **Full Name Split** (full_name column was combined → split into first_name & last_name to enable Union All):
  - `first_name:` `SUBSTRING(full_name,1,FINDSTRING(full_name," ",1)-1)`
  - `last_name:` `SUBSTRING(full_name,FINDSTRING(full_name," ",1)+1,LEN(full_name))`
- **Date (format: MM/DD/YYYY → YYYY-MM-DD):** same as Excel

### Source 4 — JSON (.json)
- Loaded via **Script Component** (C#) since SSIS has no native JSON source
- Parsed and mapped to match the unified schema before Union All

> All 4 sources are unified via **Union All** transformation and loaded into a single **ODS Database**.

## 📦 SSIS Packages

### ODS.dtsx — Data Ingestion
- **Sources:** Excel, Flat File (CSV), XML, Script Component (JSON)
- **Transformations:** Derived Column, Data Conversion, Union All
- **Destination:** OLE DB → Single ODS Database

### STG.dtsx — Staging Layer
- Execute SQL Task → creates staging tables
- Sequence Container (DIM_Sequence):
  - load Dim_emp → Dim_date → Dim_gender → Dim_dept → Dim_loc → OBT_TABLE

### DWH.dtsx — Data Warehouse
- Execute SQL Task → creates DWH schema
- **DIM_LOAD:** DIM_dept, DIM_gender, DIM_date, DIM_emp, DIM_loc
- **FACT_LOAD:** Lookup chain → gender → dept → emp → loc → date → AGE_COL → OLE DB Destination

## 🗃️ Star Schema
- **Fact Table:** Employee Fact
- **Dimensions:** Dim_Employee, Dim_Gender, Dim_Department, Dim_Location, Dim_Date

## 🛠️ Tech Stack
![SSIS](https://img.shields.io/badge/SSIS-CC2927?style=flat&logo=microsoft-sql-server&logoColor=white)
![SQL Server](https://img.shields.io/badge/SQL_Server-CC2927?style=flat&logo=microsoft-sql-server&logoColor=white)
![Visual Studio](https://img.shields.io/badge/Visual_Studio-5C2D91?style=flat&logo=visual-studio&logoColor=white)

## 🔗 Connect
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/shahd-safwat-mohamed-965213262)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/Shahdsafwat)


