# Azure End-to-End Data Engineering Project



## Project Overview

This project focuses on building a data engineering solution on Azure, encompassing data ingestion, processing, warehousing, and basic visualisation. The architecture follows the **Medallion architecture** (Bronze, Silver, Gold layers) to organise and refine data as it moves through the pipeline .

The project ingests data from a **GitHub API** , orchestrates the data flow using **Azure Data Factory** with dynamic pipelines , performs transformations using **Azure Databricks** with Spark , serves the data through **Azure Synapse Analytics** as a data warehouse , and briefly demonstrates connection to **Power BI** for visualisation .

## Key Technologies Used

*   **Azure Data Factory (ADF):** A powerful orchestration tool used to build dynamic pipelines for data ingestion and movement . Key features covered include building dynamic pipelines with parameters and loops .
*   **Azure Databricks:** A data analytics platform based on Apache Spark, used for performing data transformations and analysis on data in the Bronze layer before moving it to the Silver layer .
*   **Azure Synapse Analytics:** A fully managed data warehouse service used as the Gold layer to serve cleansed and transformed data to stakeholders . The project utilises the **serverless SQL pool** in Synapse Analytics .
*   **Azure Blob Storage (Data Lake Gen2):** Used as the storage layer for Bronze, Silver, and Gold zones following the Medallion architecture .
*   **Power BI:** Briefly used to demonstrate how to establish a connection with Azure Synapse Analytics to visualise the data .
*   **Azure Active Directory (Microsoft Entra ID):** Used to create a service principal for secure data access from Azure Databricks to the Data Lake .
*   **Managed Identities:** Leveraged for secure access between Azure Synapse Analytics and the Data Lake .
*   **HTTP Connector:** Used in Azure Data Factory to connect to the GitHub API as the data source .

## Project Architecture

The data flow in this project follows these stages :

1.  **Data Source:** Data is pulled from the **GitHub API**.
2.  **Bronze Layer (Raw):** Raw data is ingested into the Azure Data Lake Storage Gen2 using Azure Data Factory without any transformations .
3.  **Silver Layer (Transformed):** Data is read from the Bronze layer by Azure Databricks, transformations are applied using Spark, and the refined data is stored in the Silver layer in Parquet format .
4.  **Gold Layer (Serving):** Data from the Silver layer is loaded into Azure Synapse Analytics to create a data warehouse ready for consumption by data analysts and other stakeholders . Serverless SQL pool is used to query the data in the Data Lake .
5.  **Data Visualisation:** A connection is established between Azure Synapse Analytics and Power BI to visualise the data .

## Setup Instructions

To set up and run this project, you will need an **Azure subscription** . You can create a **free Azure account** by following the steps outlined in the video .

Here's a high-level overview of the setup process:

1.  **Create an Azure Account:** Follow the instructions in the video to create a free Azure account .
2.  **Create a Resource Group:** Create an Azure Resource Group to contain all the project resources .
3.  **Create Azure Data Lake Storage Gen2 Account:** Create a storage account configured for Data Lake Gen2 to store the Bronze, Silver, and Gold layer data [17]. Create containers within this storage account (e.g., `bronze`, `silver`, `gold`, `parameters`) .
4.  **Create Azure Data Factory:** Create an Azure Data Factory instance for orchestrating the data pipeline .
5.  **Configure Linked Services in ADF:**
    *   Create an **HTTP linked service** to connect to the GitHub API .
    *   Create an **Azure Data Lake Storage Gen2 linked service** to connect to your storage account .
6.  **Create Datasets in ADF:**
    *   Create a parameterized dataset for the HTTP source .
    *   Create parameterized datasets for the Bronze layer in the Data Lake .
7.  **Build Dynamic Pipelines in ADF:** Implement a pipeline that uses a **Lookup activity** to read a JSON file containing metadata about the files to ingest, a **ForEach activity** to iterate through the files, and a **Copy activity** within the ForEach loop to load data dynamically from the GitHub API to the Bronze layer .
8.  **Create Azure Databricks Workspace:** Create an Azure Databricks workspace to perform data transformations . Create a cluster within the workspace.
9.  **Configure Data Access from Databricks:**
    *   Register an **application in Azure Active Directory (Entra ID)** .
    *   Create a **client secret** for the application .
    *   Assign the **"Storage Blob Data Contributor" role** to the registered application on your Data Lake Storage Gen2 account.
    *   Use the application ID, tenant ID, client secret, and storage account details in a Databricks notebook to configure access to the Data Lake .
10. **Develop Databricks Notebook:** Write PySpark code in a Databricks notebook to read data from the Bronze layer, perform necessary transformations, and write the transformed data to the Silver layer in Parquet format .
11. **Create Azure Synapse Analytics Workspace:** Create an Azure Synapse Analytics workspace .
12. **Grant Synapse Workspace Access to Data Lake:** Assign the **"Storage Blob Data Contributor" role** to the **managed identity** of your Synapse Analytics workspace on your Data Lake Storage Gen2 account . Also, assign the same role to your own user account for querying data .
13. **Create a Serverless SQL Pool Database in Synapse:** Create a database in the serverless SQL pool within your Synapse workspace .
14. **Create External Data Sources, Credentials, and File Formats in Synapse:** Configure external resources to access data in the Silver layer .
15. **Create Views in Synapse:** Create SQL views on top of the data in the Silver layer using the `OPENROWSET` function .
16. **Create External Tables in Synapse (Optional):** Demonstrate creating external tables using `CREATE EXTERNAL TABLE AS SELECT (CETAS)` to move data to the Gold layer in Parquet format .
17. **Connect Power BI to Azure Synapse Analytics:** Use the **serverless SQL endpoint** of your Synapse workspace to establish a connection in Power BI Desktop using database credentials (admin user and password set during Synapse workspace creation) .
