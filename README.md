# 🟦 Azure E-Commerce Data Platform (Batch + Real-Time)

<p align="center">
  <img src="https://img.shields.io/badge/Azure-Cloud-blue?logo=microsoftazure&logoColor=white" alt="Azure"/>
  <img src="https://img.shields.io/badge/Synapse-Analytics-blue?logo=apache-spark&logoColor=white" alt="Synapse"/>
  <img src="https://img.shields.io/badge/Python-3.10-yellow?logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/ADF-Data%20Factory-blue?logo=microsoftazure&logoColor=white" alt="ADF"/>
  <img src="https://img.shields.io/badge/EventHub-Streaming-blue?logo=microsoftazure&logoColor=white" alt="Event Hub"/>
  <img src="https://img.shields.io/badge/LogicApps-Integration-blue?logo=microsoftazure&logoColor=white" alt="Logic Apps"/>
  <img src="https://img.shields.io/badge/ADLS-Gen2-blue?logo=microsoftazure&logoColor=white" alt="ADLS Gen2"/>
</p>

## 📌 Project Overview

This project implements a real-world e-commerce data platform using Azure services.  
It includes both **batch ingestion** and **real time streaming** ingestion and follows the **Medallion Architecture** (RAW → BRONZE → SILVER → GOLD).

The data source is **FakeStore API**, and the platform ingests:

- Products (batch)
- Users (batch)
- Carts/Orders (real-time streaming via Event Hub)

This project demonstrates end-to-end Data Engineering skills:  
✔ Ingestion  
✔ Streaming  
✔ Data transformation  
✔ Lakehouse architecture  
✔ Notebook orchestration  
✔ Security & role configuration  
✔ Production-ready folder structure

## 🏗 High-Level Architecture

<p align="center">
  <img src="architecture/high_level_architecture.png" width="250" alt="High-Level Architecture"/>
</p>

## 🧱 Medallion Architecture

<p align="center">
  <img src="architecture/medallion_architecture.png" width="250" alt="Medallion Architecture"/>
</p>

## ⚡ Streaming Architecture

<p align="center">
  <img src="architecture/streaming_flow.png" width="250" alt="Streaming Architecture"/>
</p>

## 🔄 Notebook Orchestration Flow

<p align="center">
  <img src="architecture/orchestration_flow.png" width="250" alt="Notebook Orchestration Flow"/>
</p>

## 📁 Repository Structure
azure-ecommerce-data-platform/
│
├── architecture/          # Architecture diagrams
│   ├── high_level_architecture.png
│   ├── medallion_architecture.png
│   ├── streaming_flow.png
│   └── orchestration_flow.png
│
├── notebooks/             # Synapse Spark notebooks
│   ├── 01_nb_bronze_batch_load.ipynb
│   ├── 02_nb_bronze_stream_load.ipynb
│   ├── 03_nb_silver_transform.ipynb
│   ├── 04_nb_silver_order_details.ipynb
│   └── 05_nb_gold_metrics.ipynb
│
├── screenshots/           # Project screenshots
│   ├── adf_pipeline.png
│   ├── adf_synapse_orchestration.png
│   ├── adls_foldertree.png
│   ├── bronze_stream_folder.png
│   ├── eventhub_dashboard.png
│   ├── gold_layer_preview.png
│   ├── logicapp_designer.png
│   ├── logicapp_runhistory.png
│   ├── silver_layer_preview.png
│   ├── synapse_notebooks.png
│   └── synapse_stream_output.png
│
├── pipelines/             # ADF ARM templates
│   ├── PL_FakeStore_Ingestion.json
│   └── PL_Synapse_Notebook_Runner.json
│
├── challenges.md          # Challenges & solutions document
└── README.md              # Project documentation


## 📥 Batch Ingestion (ADF)

✅ **ADF Pipeline: PL_FakeStore_Ingestion**  
This pipeline fetches:

- `/products`
- `/users`

from FakeStore API and stores them in:

/raw/fakestore/products
/raw/fakestore/users


<p align="center">
  <img src="screenshots/adf_pipeline.png" width="600" alt="ADF Pipeline"/>
</p>

## 🔐 Azure Role Assignments (Very Important)

Your project required multiple permissions:

1. **ADLS → ADF Permissions**  
   To allow copying data:  
   - Storage Blob Data Contributor  
   - Storage Blob Data Reader

2. **Synapse → ADF Triggering**  
   To trigger Notebooks:  
   - Synapse Administrator  
   - Synapse Apache Spark Administrator  

   ADF’s service principal was granted these roles.

3. **Event Hub Permissions**  
   - Logic App → Send  
   - Synapse → Listen  
   - Testing → Manage

4. **Synapse Workspace User**  
   You (the developer) had:  
   - Synapse Administrator  
   - Storage Blob Data Contributor

## 🧩 Challenges (summarized)

A full list is in [`challenges.md`](challenges.md), but key items include:  
- 🔸 Event Hub "Unauthorized" Error → Solved by moving access policy from Namespace level to Entity level.  
- 🔸 Notebook Not Appearing in ADF → Solved by assigning ADF service principal Synapse Administrator + Spark Administrator roles.  
- 🔸 Streaming JSON Flattening → Fixed using `explode(col("products"))`  
- 🔸 Join type mismatch in Silver layer → Resolved by casting `col("product_id").cast("int")`

## 🧪 How to Run This Project

1. Deploy ADLS Gen2 → Create container `datalake` with folders: `raw/`, `bronze/`, `silver/`, `gold/`  
2. Deploy ADF and configure linked services.  
3. Deploy Logic App (1-minute recurrence).  
4. Deploy Event Hub (1 consumer group).  
5. Deploy Synapse Workspace + Spark Pool.  
6. Upload all notebooks.  
7. Create Notebook Orchestration ADF Pipeline.  
8. Check Bronze → Silver → Gold outputs.

## 🏁 Conclusion

This project demonstrates:  
- Batch & streaming ingestion  
- ADLS Medallion architecture  
- Spark-based transformations  
- Event-driven design  
- Data engineering best practices  
- Production-grade Azure setup
