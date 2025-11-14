# MidTermProject
Food Inspection Analytics – Medallion Architecture Project

📌Project Overview
This project implements a full end-to-end data engineering pipeline using:
Alteryx → raw data profiling
Databricks Lakehouse → Bronze → Silver → Gold Medallion Architecture
Delta Live Tables (DLT) → streaming ingestion, transformations, expectations
SCD Type 2 → for Business dimension
Star Schema / Dimensional Modeling → fact + dimension tables
Power BI Dashboard 

The dataset includes Food Inspection Records from Chicago and Dallas, processed independently by team members but designed to align into a unified dimensional model.


📁 Folder Structure: 
/alteryx_profiling/
    Chicago_Profiling.yxmd
    Dallas_Profiling.yxmd

/databricks/
    /bronze/
    /silver/
    /scd2_chicago/
    /gold/
/dashboards/

/docs/
    ERD.png
    DataMappingTemplate.xlsx
    Assignment_Requirements.pdf

/README.md


🏗️ Medallion Architecture Summary: 
Bronze Layer
•	Raw CSV files from Chicago and Dallas loaded using cloud_files.
•	Change Data Feed (delta.enableChangeDataFeed = true) enabled for incremental loads.
•	No transformations applied.

Silver Layer- Chicago -> 
Cleansing rules based on  requirements:
✔ Restaurant Name cannot be NULL
✔ Inspection Date cannot be NULL
✔ Inspection Type cannot be NULL
✔ ZIP Code must be non-null and match ^[0-9]{5}$
✔ City must be CHICAGO
✔ Results cannot be NULL
✔ Violations must contain at least one unique violation
✔ Violations parsed into violation_code and violation_description
✔ Duplicate violations removed (SELECT DISTINCT)
✔ One-to-many explosion handled using explode(split(...))


Dallas — Silver->
•	Validates score fields
•	Validates inspection type & date
•	Violations already structured, so no regex needed
•	ZIP, business info, facility info cleaned
•	Restaurant Name cannot be NULL
•	Inspection Date cannot be NULL
•	Inspection Type cannot be NULL
•	In Dallas dataset –
• if violation score is 90 or more than we should not have more than 3 violation associated
• Inspection result cannot be PASS if any of the violation contains Urgent/Critical terms


GOLD LAYER
Create dimensions, facts, and surrogate keys for BI / reporting.
City: Chicago,	What Gold Does: Full star schema, aggregated violations, date_dim added
City:Dallas, 	What Gold Does: Similar star schema with structured violations


SCD TYPE 2
Track historical changes in dimension attributes.
Implemented for: Business Dimension 


📌 Source to Target Mapping
Source → Silver → Gold
Includes:
•	Data types
•	Business rules applied
•	Keys (NK, SK, FK)
•	Transformation logic


📊 Power BI Dashboard
Dataset export came from Gold tables in Databricks.


🏁 Final Statement
This project successfully demonstrates a full Medallion Architecture implementation on Databricks with:
•	Streaming ingestion
•	Cleansing transformations
•	Dimensional modeling
•	SCD Type 2 handling
•	Validation frameworks
•	BI-ready Gold tables
The Chicago and Dallas pipelines follow the same modeling strategy, allowing the final Power BI dashboard to present a unified view across both cities.



👥 Team Members: 
1) Rakshith Narayanswamy
2) Radhika Joshi 
3) Akshay Raj Chevala

