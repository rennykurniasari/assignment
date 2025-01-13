# GCP Data Pipeline Architecture for Task 3

## **Architecture Diagram**

![GCP Data Pipeline Architecture](https://drive.google.com/file/d/15fLtnt1SzA4fWATsl_eJ0C8w39yYf8Z1/view?usp=drive_link)

## **Components Overview**

This architecture leverages GCP services to create a scalable, efficient data pipeline for ingesting, transforming, and processing transactional data. The pipeline supports both Data Analytics and Data Science teams.

### **1. Data Ingestion**
- **Cloud Storage:**
  - Role: Acts as the landing zone for raw data. Data is uploaded hourly (e.g., `20241031/14/users.csv`).
  - Storage Structure: Raw data is organized by folders for date and hour.
- **Pub/Sub:**
  - Role: Publishes events when new files are uploaded to Cloud Storage. Triggers downstream processing pipelines.

### **2. Data Transformation**
- **Cloud Dataflow:**
  - Role: Processes raw data into structured formats using Apache Beam pipelines. The processed data is cleaned, aggregated, and written to BigQuery staging tables.
  - Examples:
    - Calculates Total Purchase Value (TPV) and aggregates transactions.
    - Prepares data for Fact and Dimension tables.

### **3. Data Storage and Modeling**
- **BigQuery:**
  - Role: Serves as the primary data warehouse. Stores:
    - **Staging Tables:** Temporary cleaned and raw data.
    - **Fact and Dimension Tables:** Final structured tables for analytics and modeling (e.g., Fact Transaction Table, User Dimension Table, Goods Dimension Table).
  - Benefits: Scalable, fast SQL-based analytics, and integration with other GCP services.

### **4. Orchestration**
- **Cloud Composer (Apache Airflow):**
  - Role: Schedules and orchestrates pipeline workflows, including:
    - Hourly ingestion of new data.
    - Transformation and loading into BigQuery.
    - Notifying teams upon pipeline completion.

### **5. Data Access**
- **Data Analytics:**
  - **Looker Studio (formerly Data Studio):**
    - Role: Provides dashboards and visualizations for analytics teams.
    - Directly connects to BigQuery to query Fact and Dimension tables.
- **Data Science:**
  - **Vertex AI:**
    - Role: Trains and deploys machine learning models (e.g., anomaly detection for transactions).
    - Uses BigQuery ML to query data hourly and run ML models efficiently.

### **6. Monitoring and Logging**
- **Cloud Logging:**
  - Role: Tracks pipeline events, errors, and performance metrics.
- **Cloud Monitoring:**
  - Role: Provides insights into the health of the pipeline and its components, sending alerts when anomalies are detected.

### **7. Security**
- **IAM (Identity and Access Management):**
  - Role: Manages permissions for accessing GCP resources, ensuring only authorized users and services have access.
- **Cloud KMS (Key Management Service):**
  - Role: Manages encryption keys for securing data in transit and at rest.

---

## **Pipeline Workflow**
1. **Data Upload:**
   - Transactional data is uploaded to Cloud Storage in the raw format.
2. **Event Trigger:**
   - Pub/Sub detects new files and triggers a Dataflow job for processing.
3. **Data Transformation:**
   - Dataflow cleans, aggregates, and writes processed data into BigQuery staging tables.
4. **Data Modeling:**
   - dbt models transform staging data into Fact and Dimension tables in BigQuery.
5. **Data Consumption:**
   - Analytics teams use Looker Studio for visualization.
   - Data Science teams use Vertex AI for anomaly detection and modeling.
6. **Monitoring and Alerts:**
   - Logs and metrics are monitored using Cloud Logging and Cloud Monitoring.

---

## **Key Features**
- **Scalability:** Leveraging BigQuery and Cloud Dataflow ensures the pipeline can handle increasing data volumes.
- **Real-Time Processing:** Pub/Sub and Dataflow enable near-real-time ingestion and transformation.
- **Seamless Integration:** GCP services like BigQuery, Looker Studio, and Vertex AI work together efficiently.
- **Security:** IAM and Cloud KMS ensure secure data handling.
- **Automation:** Cloud Composer automates the entire pipeline, reducing manual intervention.

---
