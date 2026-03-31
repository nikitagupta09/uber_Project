# 🚖 Uber Real-Time Data Engineering Project

## 📌 Overview
This project simulates an Uber ride booking system and builds an end-to-end Azure data pipeline using both real-time streaming and batch ingestion. Ride events are generated through a web application, streamed through Azure Event Hub, processed in Databricks using PySpark, and modeled into Bronze, Silver, and Gold layers for analytics.

---

## 🧠 Architecture

![Architecture](architecture.png)

### Main Flow
- **WebApp → FastAPI + Jinja2 → Azure Event Hub → Databricks**
- **GitHub / JSON files → Azure Data Factory (ADF) → ADLS Gen2 → Databricks**
- **Databricks → Bronze → Silver (OBT) → Gold (Star Schema)**

---

## ⚙️ Tech Stack
- **Frontend:** HTML, Jinja2 Templates
- **Backend/API:** FastAPI
- **Version Control:** GitHub
- **Streaming Platform:** Azure Event Hub
- **Streaming Protocol:** Kafka-compatible Event Hub connection
- **Batch Ingestion / Orchestration:** Azure Data Factory (ADF)
- **Storage:** Azure Data Lake Storage Gen2 (ADLS Gen2)
- **Processing Engine:** Azure Databricks
- **Data Processing:** PySpark, Spark Structured Streaming
- **Languages:** Python, SQL
- **Data Generation:** Faker

---

## 🚀 Pipeline Breakdown

### 1. WebApp to Event Hub
- A user books a ride from the web application
- The FastAPI backend renders pages using **Jinja2**
- Ride data is generated dynamically using **Faker**
- The event is published to **Azure Event Hub**

### 2. Event Hub to Databricks
- Databricks consumes streaming ride events from Event Hub
- Event Hub is read using its **Kafka-compatible interface**
- Streaming data is processed using **PySpark Structured Streaming**

### 3. ADF to Data Lake
- Reference files and initial load files are managed through **GitHub / JSON sources**
- **ADF** ingests these files into **ADLS Gen2**
- This supports batch ingestion for lookup and initial datasets

### 4. ADLS Gen2 to Databricks
- Databricks reads batch data from **ADLS Gen2**
- Initial load data and mapping files are used in downstream processing

---

## 🥉 Bronze Layer
The Bronze layer stores raw and staging data from both ingestion paths.

- Streaming ride data from Event Hub
- Batch / reference data from ADLS Gen2
- Raw JSON is parsed into staging structures such as `stg_rides`

---

## 🥈 Silver Layer
The Silver layer creates a cleaned and enriched streaming model.

- Initial load and streaming data are combined into a streaming table
- Ride data is joined with mapping / lookup tables
- A **One Big Table (OBT)** is created as `silver_obt`

This layer standardizes and enriches ride records before dimensional modeling.

---

## 🥇 Gold Layer
The Gold layer transforms the OBT into a Star Schema for analytics.

### Fact Table
- Ride metrics such as:
  - fare
  - distance
  - duration
  - tip
  - rating

### Dimension Tables
- `dim_passenger`
- `dim_driver`
- `dim_vehicle`
- `dim_payment`
- `dim_booking`
- `dim_location`

---

## 🔄 Data Flow
- **Streaming Path:** WebApp → FastAPI/Jinja2 → Event Hub → Databricks → Bronze → Silver → Gold
- **Batch Path:** GitHub / JSON → ADF → ADLS Gen2 → Databricks → Bronze → Silver → Gold

---

## 🎯 Key Features
- Real-time event streaming with Azure Event Hub
- Kafka-based stream consumption in Databricks
- Batch ingestion with Azure Data Factory
- Storage in ADLS Gen2
- PySpark-based transformation pipeline
- Medallion architecture: Bronze, Silver, Gold
- One Big Table (OBT) design in Silver
- Star Schema modeling in Gold
- Hybrid architecture supporting both streaming and batch ingestion

---

## ▶️ Run
`uvicorn api:app --reload`

---

## 🏁 Summary
This project demonstrates a hybrid Azure data engineering architecture for Uber ride data using FastAPI, Jinja2, Azure Event Hub, Kafka-compatible streaming, ADF, ADLS Gen2, Databricks, PySpark, and Star Schema modeling for analytics.
