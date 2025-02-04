# fitness_app_spark_streaming

## Project Overview:

This project focuses on building a **Lakehouse data pipeline** to process fitness data efficiently. 
I will implement a **structured data flow (Bronze, Silver, Gold layers)** using **both batch processing and Spark Streaming** to handle real-time and historical data efficiently. The pipeline will ingest and process **user workout and gym activity data** as the goal.

## Technology Used:

1. Programming Language - Python, PySpark, Spark Structured Streaming
2. Scripting Language - SQL
3. Microsoft Azure
    - Azure Data Lake Storage
    - Azure Databricks with Unity Catalog
    - Azure DevOps

## Requirements:

- **Lakehouse Architecture** – Implement a **Lakehouse platform** using the **medallion architecture** (Bronze, Silver, Gold) for structured data storage and processing.
- **Data Ingestion** – Collect and ingest **fitness data** (workouts, BPM, logins) from APIs, databases, and Kafka, supporting both **batch and streaming workflows**.
- **Data Processing** – Transform raw data into **cleaned and aggregated datasets** using **Databricks, PySpark, and Delta Lake**, ensuring **data quality and efficiency**.
- **Analytics & Reporting** – Prepare **Workout BPM Summary** and **Gym Summary** datasets for **insights, dashboards, and reporting**.
- **Security & Automation** – Implement **role-based access control (RBAC) from Unity Catalog**, **CI/CD pipelines**, and **automated testing** for deployment and data validation.
- **Scalability & Performance** – Design for **high availability, scalability, and cost efficiency**, optimizing queries and storage for **real-time and batch processing**.

## About the datasets:

![image.png](attachment:84acbcae-4d2f-40b1-a9c6-460ba749da00:image.png)

There are 5 datasets that will be ingested into landing area (data_zone) directory within azure data lake. 

1. Registration - contains data for registered users (User ID, Device ID, Mac Address, Registration Timestamp)
2. User Profile - contains data for user profile like name, dob, sex, address, and update_type. The update_type columns will indicate whether the users are updated or removed.
3. BPM Stream - contains data for heartrate (bpm) for user. (device_id, time, heartrate)
4. Login/Logout - contains data regarding the time user login or logout from the gym. (Mac Address, Gym, Login, Logout)
5. Workout Session - contains data of every workout session for users. (user_id, workout_id, timestamp, action, session_id)

Datasets are available on my github.

## Databricks Lakehouse Platform

![image.png](attachment:9d32e69c-7355-4892-ad14-5d7b4d8ccb4f:image.png)

Using ADLS to create a container. And has 3 directories called

/sbit-metastore-root to store metadata of the project when using Unity Catalog

/sbit-managed-dev to store data from bronze, silver and gold databases

/sbit-unmanaged-dev to store raw data and checkpoint for streaming

## Data Governance and Security using Unity Catalog

Implement Data Security for directories using Unity Catalog:

1. Do not grant direct access to the data directories for everyone
2. Grant read only permission for data zone (landing-zone) directory.
3. Grant read/write permission on the bronze_db, silver, and gold databases.
4. Grant read only permission for checkpoint zone directory.
5. Implement resource policies by limiting cluster type and size, number of cluster per users, costs, configurations and autoscaling capacity.

## Data Architecture Design

![image.png](attachment:1537a28f-becb-4bb7-9489-f55e24bab110:image.png)

## How to run project notebook

There are 10 notebooks that I have created for the project:

01-config - contains configuration to get the path of data lake as external location

02-setup - contains code for setting up 5 landing zone tables

03-history-loader - contains code to create a table with dates (days, weeks, months, year) for merging purposes with landing zone tables

04-bronze - contains ETL process from landing zone to bronze layer tables

05-silver - contains ETL process from bronze to silver layer tables

06-gold - contains ETL process from silver to gold layer tables

07-run - contains the script to run all notebooks

08-batch-test - testing and QC code for batch process

09-stream-test - testing and QC code for streaming process

10-producer - produce raw data sets by moving it to landing zone

run notebook 08 to run the pipeline in batch mode

run notebook 09 to run pipline in streaming mode
