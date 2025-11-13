# AzureDatafactory_pipeline

# 🧠 Azure Data Pipeline with Data Factory & Databricks

This project was a great opportunity for me to design and implement a **complete data pipeline** on **Microsoft Azure**, integrating **Azure Data Factory**, **Azure Databricks**, and **Azure Data Lake Gen2**.  

It simulates a real-world **ETL process** — from raw data ingestion to transformation and automation — following the **Bronze** and **Silver** data lake architecture.

---

## 🚀 Project Overview

In this project, I learned how to:

- Configure and manage Azure resources efficiently;
- Create and secure an **App Registration** for authentication;
- Build a **Databricks workspace** to process data;
- Structure a **Data Lake Gen2** with proper layers;
- Develop and orchestrate an **ETL pipeline** in **Azure Data Factory**;
- Automate execution through **triggers**.

---

## 🏗️ Architecture


Data Source → Data Lake (Inbound) → Databricks (Bronze & Silver Layers) → Azure Data Factory (Orchestration)



##🧩 Layers Overview
Inbound → Raw incoming data

Bronze → Cleaned data saved in Delta format

Silver → Transformed and structured data ready for analysis




## ⚙️ Steps and Implementation

1️⃣ Configure Azure Resources

To start, I created my Azure account and set up:

A resource group to organize all related assets;

A cost alert to monitor usage and prevent unexpected billing.

2️⃣ Register Application and Assign Permissions

To securely connect all services, I created an App Registration in Azure.

🔐 App Registration

This provides credentials like:

client_id

tenant_id

client_secret

These are required for Databricks and Data Factory to authenticate when accessing Azure resources.

🧾 Permissions

Access control was managed through:

IAM (Identity and Access Management)

ACL (Access Control List)

Both tools were configured to define the correct level of access for each service to the Data Lake containers.

3️⃣ Create and Structure the Data Lake

I created a Data Lake Gen2 using Azure Storage Account and organized it into three main layers:

data-lake/
├── inbound/
├── bronze/
└── silver/


Then, I uploaded the initial dataset (real estate data) into the inbound layer, which serves as the raw input source.

4️⃣ Configure Databricks Workspace

Next, I deployed a Databricks workspace directly from Azure.
This environment is responsible for running notebooks that perform all data transformations throughout the pipeline.

5️⃣ Mount Access to the Data Lake

To allow Databricks to read and write data from the lake, I mounted the storage using the credentials from my App Registration.

configs = {
    "fs.azure.account.auth.type": "OAuth",
    "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
    "fs.azure.account.oauth2.client.id": "<application-id>",
    "fs.azure.account.oauth2.client.secret": "<secret>",
    "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"
}

dbutils.fs.mount(
  source = "abfss://<container>@<storage-account>.dfs.core.windows.net/",
  mount_point = "/mnt/data_lake",
  extra_configs = configs
)

6️⃣ Transform Data and Save to Bronze Layer

At this stage, I performed initial cleaning and transformation tasks:

Removed irrelevant columns (e.g., images, user details);

Ensured a unique identifier (id) for each record;

Saved the output in Delta format within the Bronze layer.

7️⃣ Transform Data and Save to Silver Layer

I then processed the Bronze data to create a more structured Silver dataset:

Flattened JSON fields into individual columns;

Removed unnecessary descriptive fields;

Stored the result in Delta format in the Silver layer.

8️⃣ Create and Configure Azure Data Factory

To orchestrate and automate the data flow, I set up an Azure Data Factory instance.
Here, I connected to my Databricks workspace and created:

Linked Services

Datasets

Pipelines

These components were configured to manage and execute Databricks notebooks directly from ADF.

9️⃣ Develop and Configure the Pipeline

Within Azure Data Factory, I designed a pipeline responsible for:

Executing Databricks notebooks;

Moving data through the layers (Inbound → Bronze → Silver);

Ensuring data consistency and execution dependencies.

🔟 Test the Pipeline Execution

Before automating everything, I ran several tests to validate:

The pipeline’s full execution path;

Data integrity across layers;

Connection between ADF and Databricks.

1️⃣1️⃣ Create an Execution Trigger

Since new property data is added to the Data Lake every hour, I created a trigger in Azure Data Factory to automatically run the pipeline whenever new files are detected.
This ensures continuous data updates without manual intervention.





## 🧾 Technologies Used

Tool	Purpose
🟦 Azure Data Factory	Pipeline orchestration & automation
🔷 Azure Databricks	Data transformation & processing
🪣 Azure Data Lake Gen2	Layered data storage
🔐 Azure Active Directory	Authentication & access management
🐍 Python / PySpark	Data transformation scripts



## 🧠 Key Learnings
Throughout this project, I learned how to:

Design a layered data lake architecture (Bronze, Silver, Gold);

Integrate Databricks and Data Factory using App Registration;

Mount and access Data Lake Gen2 securely;

Automate pipelines and manage data triggers in Azure.



## 💡 Future Improvements

Add a Gold layer for analytics-ready data

Integrate with Power BI or Azure Synapse Analytics for visualization

Implement CI/CD with Azure DevOps or GitHub Actions
