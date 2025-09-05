# **Enterprise Data Warehousing using Amazon Redshift By Nithin Suresh**  

## **Problem Statement**  
Design and implement a scalable **data warehousing solution** for a retail business, leveraging **AWS S3, Redshift, and ETL pipelines**. The goal is to transform raw transactional data into an optimized **Star Schema** that supports historical tracking (SCD Type 2), efficient querying, and advanced business insights.  

---

## **Project Workflow**  

### **1. Relational Schema Design (OLTP Layer)**  
Modeled a **normalized relational schema** to capture day-to-day retail transactions:  
- **Customers**: (CustomerID, FirstName, LastName, Email, Address, City, State, ZipCode, LastModified)  
- **Products**: (ProductID, ProductName, Category, Description, Price, LastModified)  
- **Stores**: (StoreID, StoreName, Address, City, State, ZipCode, LastModified)  
- **Orders**: (OrderID, CustomerID, StoreID, OrderDate, TotalPrice, LastModified)  
- **OrderDetails**: (OrderID, ProductID, Quantity, Price, LastModified)  

This schema reflects the transactional source system (OLTP) and ensures data consistency.  

---

### **2. Data Generation & Population**  
- Generated **synthetic yet realistic datasets** using ChatGPT-driven data generation and Python scripts.  
- Produced CSV files for each table, ensuring **referential integrity** across relationships (e.g., Orders referencing Customers, OrderDetails referencing Products).  

---

### **3. Data Ingestion into S3 (Data Lake Layer)**  
- Provisioned an **Amazon S3 bucket** as the raw data landing zone.  
- Organized data into directories by table name for versioned uploads (e.g., `/customers/`, `/orders/`).  
- Each update cycle pushes a new file, preserving a full **immutable data history** in S3.  

---

### **4. Data Loading into Redshift (Staging Layer)**  
- Launched a **Redshift cluster** with proper VPC, IAM, and security group configurations.  
- Created staging tables mirroring the relational schema.  
- Used the **Redshift COPY command** for high-performance parallel ingestion of CSVs from S3 into staging.  

---

### **5. Data Transformation into Star Schema (Warehouse Layer)**  
- Modeled a **Star Schema** optimized for analytics:  
  - **FactSales** (Fact table: sales transactions)  
  - **DimCustomer** (Slowly Changing Dimension Type 2 for historical customer changes)  
  - **DimProduct** (Product catalog with versioning support)  
  - **DimStore** (Store details with historical tracking)  
  - **DimDate** (Calendar dimension for time-series analysis)  
- Built **SCD Type 2 logic** in SQL to handle updates and maintain historical accuracy.  
- Implemented **Upsert pipelines** via staging tables:  
  - New records → Inserted.  
  - Updated records → Versioned with validity dates.  

---

### **6. Analytical Insights (BI Layer)**  
Leveraged Redshift SQL for advanced analytics and business KPIs:  
- **Top 10 Products:** Ranked by total sales volume (Quantity Sold).  
- **Monthly Sales Trend:** Aggregated revenue per month, enabling YOY/seasonality analysis.  
- **Store Performance:** Ranked stores by total revenue to identify best/worst performers.  
- **Seasonal Analysis:** Quarterly revenue trends to uncover high-sales periods.  
- **Average Order Value (AOV):** Calculated total revenue ÷ total orders, monitored over time.  
- **Geo Insights:** Sales performance by **city and state**, highlighting regional strengths.  

---

### **7. Data Pipeline Automation**  
- Designed an **end-to-end ETL pipeline** using AWS Glue and Python scripts:  
  - **Extract:** Ingest CSVs from S3.  
  - **Transform:** Apply SCD Type 2 and star schema modeling in staging/warehouse layers.  
  - **Load:** Upsert into fact and dimension tables in Redshift.  
- Automated scheduling with **AWS Glue Jobs** and **EventBridge** for seamless refreshes.  
- Ensured data lineage, scalability, and reproducibility for production-grade use cases.  



!
