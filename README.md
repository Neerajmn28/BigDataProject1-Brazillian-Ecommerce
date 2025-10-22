# Azure Data Factory – Data Pipelines and Orchestration
To automate and orchestrate the data flow from multiple sources into Azure Data Lake Storage Gen2, I designed and implemented robust Azure Data Factory (ADF) pipelines. These pipelines serve as the backbone of my data ingestion and transformation process, ensuring a reliable, scalable, and auditable data movement architecture.



<img width="3437" height="1842" alt="Image" src="https://github.com/user-attachments/assets/66236164-6c63-41b6-a926-518b8d85ef35" />

### Pipeline Overview
The Azure Data Factory pipeline consists of the following major components:
Source systems:
GitHub: For CSV datasets related to orders, payments, customers, etc.
MySQL: For structured relational data such as olist_order_payments.
MongoDB (NoSQL): For semi-structured product category metadata.
Target:
All raw data is ingested into the Bronze layer of Azure Data Lake Storage Gen2.
Transformation stage:
The ingested data is later processed and refined using Azure Databricks, resulting in cleaned and enriched datasets that populate the Silver and Gold layers of the Medallion architecture.
### Key ADF Activities Used
Lookup Activity
The Lookup activity was used to query metadata or configuration tables—helping the pipeline dynamically fetch parameters such as file names, source locations, or table schemas.
This approach enables parameterized and reusable pipelines, eliminating the need for hard-coded paths.
Example: The Lookup activity retrieves a list of source files to iterate through and ingest sequentially.
Source
The Source defines where the data originates from.
In this project, the sources were HTTP (GitHub raw CSV files), Azure Blob Storage, MySQL Database, and MongoDB via a linked service.
For SQL and MongoDB, I used linked services and datasets configured with the correct connection credentials (service principal for secure access).
Sink
The Sink represents the target destination where data is written.
The Sink in this project was Azure Data Lake Storage Gen2, specifically targeting folders in the Bronze container.
Data was written in Parquet format for optimized storage and query performance.
ADF also supports fault-tolerant retries and logging, ensuring reliability even during transient failures.
### End-to-End Flow
Ingest data from GitHub, MySQL, and MongoDB using Data Factory pipelines.
Stage all raw files in the Bronze layer of ADLS Gen2.
Transform the staged data in Azure Databricks using PySpark — cleaning, deduplicating, joining, and calculating new features (e.g., delivery delays, customer insights).
Write the refined data into the Silver layer, and aggregated analytical tables into the Gold layer.
Expose the Gold layer data via Azure Synapse Serverless SQL Pool for reporting and dashboarding.
### Medallion Architecture Recap
Layer	Purpose	Description

* 🥉 Bronze	Raw Data	Direct ingestion from GitHub, MySQL, and MongoDB without transformation — serves as an immutable data archive.

* 🥈 Silver	Cleansed Data	Data cleaned, standardized, and joined using Databricks. Redundant columns and nulls are removed.

* 🥇 Gold	Curated / Business-Ready Data	Analytical tables for reporting and machine learning; stored in Synapse for BI integration.


### Tools & Technologies Used
* Azure Data Factory – Data orchestration and pipeline management
*  Data Lake Storage Gen2 – Central data lake repository
* Azure Databricks (PySpark) – Data transformation and enrichment
* Azure Synapse Analytics – Data serving and SQL-based querying
* MySQL, MongoDB, GitHub Datasets – Source systems
* Python, Pandas, PySpark – Data processing
### Impact
This end-to-end cloud data pipeline demonstrates:
The integration of heterogeneous data sources (SQL, NoSQL)
Use of secure and scalable Azure services
Implementation of a Medallion Architecture for data maturity
Automation through Data Factory pipelines with Lookup-driven dynamic workflows
