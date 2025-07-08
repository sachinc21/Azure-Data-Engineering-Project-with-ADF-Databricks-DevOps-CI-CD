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

- **Source**: [Kaggle - Paris Olympics 2024](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0Q0amZqSUlQT1RYaGFDSVB0cnpBSmxELXM1d3xBQ3Jtc0tsajNla0pFdGpJaXNCOE0tWHVHUlNRZGtSSVRvLXFTdHQ1NnFrYURSMDJQejJVRHQyYWR1MTU3SUd0OGtwV3BhREdlang3T25sVVZrcEJ1LWlQa0h0Q2RiZnhJZkl6dVhkMDVPVVlnMTVEQ01WYk85dw&q=https%3A%2F%2Fwww.kaggle.com%2Fdatasets%2Fpiterfm%2Fparis-2024-olympic-summer-games&v=ESWqAZP2qA4)
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
  ![Screenshot 2025-05-24 202851](https://github.com/user-attachments/assets/9299bb92-1c96-4366-a867-89bce7a12b47)

### 🔹 Phase 3: CI/CD with Azure DevOps
- Setup Azure DevOps Git repository
- Implement branching strategy (main, feature branches)
- 
  ![Screenshot 2025-05-24 203352](https://github.com/user-attachments/assets/05388727-7517-494f-817e-a3063a059421)

- Create pull requests & publish to Live Mode
- 
  ![Screenshot 2025-05-24 203318](https://github.com/user-attachments/assets/86a4a81f-8ad5-413f-a871-d06f3d1095be)

- Generate ARM templates in `ADF_publish` branch

  ![Screenshot 2025-05-24 203453](https://github.com/user-attachments/assets/3ae9b84e-4572-4a4c-83d1-b234375bebeb)


### 🔹 Phase 4: Transformation (Silver Layer)
- Create Databricks notebooks using PySpark
- 
  ![Screenshot 2025-05-24 203642](https://github.com/user-attachments/assets/bd4d0e7f-71fc-4cc1-84ba-3ceead35a6d9)

- Parameterized Notebook Design

  ![Screenshot 2025-05-24 204439](https://github.com/user-attachments/assets/f1b0b9f8-0668-466f-bbc4-8e827b29f1ff)

  #### PySpark Transformations

  ```python
  df_sorted = df_sorted.withColumnRenamed("code", "athlete_id")
  df_sorted.display()
  ```
  ![Screenshot 2025-05-24 204327](https://github.com/user-attachments/assets/47998e1d-cf5b-4e07-a064-cc754e597c47)

  ```python
  df_sorted = df.sort('height', 'weight', ascending=[0,1]).filter(col('weight') > 0)
  df_sorted.display()
  ```
  
  ![Screenshot 2025-05-24 204249](https://github.com/user-attachments/assets/1a288000-6b84-40a0-8cb8-9381ad31bcde)

  ```python
  df_final.withColumn("cum_weight", sum("weight").over(Window.partitionBy("nationality").orderBy("height").rowsBetween(Window.unboundedPreceding, Window.unboundedFollowing))).display()
  ```

  ![Screenshot 2025-05-24 204408](https://github.com/user-attachments/assets/4b34725e-dc32-4777-a67f-ac49eef710b5)

- Wrote transformed data into the silver container.
```python
df_final.write.format("delta") \
        .mode("append") \
        .option("path", "abfss://silver@projdl.dfs.core.windows.net/athletes") \
        .saveAsTable("olympics.silver.athletes")
```

### 🔹 Phase 5: Curation (Gold Layer)
- Create Delta Live Tables in Databricks
- Produce business-ready curated tables
- Enable dashboard/reporting-ready outputs
  ![Screenshot 2025-05-24 171328](https://github.com/user-attachments/assets/30f08e73-b82b-460c-b43f-4bc6b06b45fb)

---

## 🛠️ CI/CD Strategy

- ✅ All ADF pipeline development was performed under feature branches.
- ✅ Merged into the main branch via pull requests.
- ✅ Changes published to ADF live mode only after peer review and approval.
- ✅ ARM templates are auto-generated in the ADF_publish branch post-publish.
