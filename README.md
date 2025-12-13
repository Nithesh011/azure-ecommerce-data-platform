📘 Azure E-Commerce Data Platform (Real-Time + Batch Lakehouse)
Built by Nithesh (GitHub: Nithesh011)

This project is a fully functional Azure Data Engineering Platform built using:

Azure Data Factory

Azure Logic Apps

Azure Event Hub

Azure Synapse Analytics (Spark)

ADLS Gen2

PySpark (Bronze → Silver → Gold)

Medallion Architecture

The platform ingests data from the FakeStore API (simulated e-commerce system) and processes it into real-time and batch analytical layers.

⭐ 1. Architecture Overview

This project reflects a real e-commerce data engineering setup that handles:

✔ Streaming ingestion (cart/order events)
✔ Batch ingestion (products & users)
✔ Medallion architecture (Bronze → Silver → Gold)
✔ Automated ETL workflows using ADF
✔ Fact table modeling
✔ Analytical gold tables for BI dashboards
🔧 High-Level Architecture Diagram

(See /architecture/high_level_architecture_dark.png)

⭐ 2. Data Flow Summary
🔹 Batch Flow

ADF fetches products & users every 3 hours

Writes to RAW → Bronze Batch → Silver → Gold

🔹 Stream Flow

Logic App sends cart/order events to Event Hub

Synapse Spark Streaming ingests events into Bronze Stream

🔹 Transformation Flow

Silver layer cleans & standardizes

Fact OrderDetails table is created

Gold layer generates business KPIs

🔹 Orchestration Flow

ADF orchestrates all batch notebooks sequentially

⭐ 3. Medallion Architecture

See /architecture/medallion_architecture_dark.png.

Bronze Layer

Raw structured & unstructured data.
Includes:

batch_products

batch_users

stream_orders

Silver Layer

Cleaned, standardized data.
Includes:

silver_products

silver_users

silver_orders

silver_fact_order_details

Gold Layer

Business-level aggregates.
Includes:

daily_revenue

product_sales

customer_sales

category_sales

⭐ 4. Notebooks Included

Path: /notebooks/

Notebook	Purpose
01_nb_bronze_batch_load	RAW → Bronze batch
02_nb_bronze_stream_load	Event Hub → Bronze stream
03_nb_silver_transform	Bronze → Silver (cleaning)
04_nb_silver_order_details	Build Fact Table
05_nb_gold_metrics	Build Gold KPIs
⭐ 5. Pipelines

Stored in /pipelines/.

ADF Pipelines:

PL_FakeStore_Ingestion → Fetch raw API data

PL_Synapse_Notebook_Orchestration

Bronze Batch → Silver → Fact → Gold

Logic App:

Calls API → Sends events to Event Hub

Spark Streaming:

Reads Event Hub continuously

Writes streaming parquet to Bronze

⭐ 6. Challenges & Solutions

Included in /challenges/.

Examples:
🔸 Event Hub Unauthorized (401)

Cause: SAS created at namespace level
Fix: Create SAS at Event Hub entity level

🔸 Logic App "Event received is null"

Cause: Send Event used outside For Each
Fix: Move inside loop + use Parse JSON

🔸 Silver join confusion

Fix: Simplified using Spark natural joins

🔸 Streaming checkpoint clarity

Explained how Structured Streaming maintains state

These demonstrate real problem-solving experience.

⭐ 7. Project Learnings (Very Important for Reviewers)

This project helped me learn:

✔ Building real-time analytics in Azure
✔ Designing Medallion Architecture
✔ Handling streaming data with checkpoints
✔ Spark partitioning behavior
✔ Orchestrating multi-layer ETL pipelines with ADF
✔ Creating fact & dimension models
✔ Debugging real Azure issues (permissions, SAS, Event Hub, ADF)

⭐ 8. How to Run This Project

Clone the repo

Configure Azure resources

Run ADF ingestion

Start Synapse streaming notebook

Execute batch pipelines via ADF

View Gold tables through Synapse/Power BI

⭐ 9. Author

Nithesh Kumar S
GitHub: Nithesh011

Cloud & Data Engineering Enthusiast
