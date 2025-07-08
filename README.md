# End-to-End Azure Data Engineering Project with CI/CD using Azure DevOps, ADF, and Databricks

## 📌 Project Overview

This project demonstrates an **end-to-end batch data pipeline** using the **Paris Olympics 2024 dataset**. It is designed to showcase **hands-on Azure cloud data engineering skills** in a production-style setup using **Git-based CI/CD pipelines**.

---

## 🧰 Tools & Technologies

| Category          | Tool                         |
|------------------|------------------------------|
| Cloud Platform   | Microsoft Azure              |
| Orchestration    | Azure Data Factory (ADF)     |
| Storage          | Azure Data Lake Gen2         |
| Processing       | Azure Databricks, PySpark    |
| Curation Layer   | Delta Live Tables            |
| DevOps & CI/CD   | Azure DevOps, Git Repos      |
| Deployment       | ARM Templates, ADF Publish   |

---

## 📦 Dataset

- **Source**: [Kaggle - Paris Olympics 2024](https://www.kaggle.com/)
- Files Used:
  - `athletes.csv`
  - `coaches.csv`
  - `events.csv`
  - `nocs.csv`

> The dataset is placed in the **source** container of the Data Lake and is transformed and stored across Bronze, Silver, and Gold zones.

---

## 🚀 Project Phases

### 🔹 Phase 1: Azure Setup
- Create Resource Group
- Create Data Lake Storage Gen2
- Create Azure Data Factory
- Create Azure Databricks Workspace

### 🔹 Phase 2: Data Ingestion (Bronze Layer)
- Use ADF to extract CSVs from GitHub & upload to Data Lake
- Transform CSV to Parquet format
- Store in Bronze container
  ![Screenshot 2025-05-24 203558](https://github.com/user-attachments/assets/b702ee92-efdb-4c14-9a7d-b9c44e23b2ef)


### 🔹 Phase 3: Transformation (Silver Layer)
- Create Databricks notebooks using PySpark
- Clean, enrich, and join data
- Store transformed data in Silver container

### 🔹 Phase 4: Curation (Gold Layer)
- Create Delta Live Tables in Databricks
- Produce business-ready curated tables
- Enable dashboard/reporting-ready outputs

### 🔹 Phase 5: CI/CD with Azure DevOps
- Setup Azure DevOps Git repository
- Implement branching strategy (main, feature branches)
- Create pull requests & publish to Live Mode
- Generate ARM templates in `ADF_publish` branch

---

## 🛠️ CI/CD Strategy

- ✅ Feature Branches for development
- ✅ Pull Requests for merging to main
- ✅ `Publish` button triggers creation of ARM templates
- ✅ Deployable code stored in `ADF_publish` branch
