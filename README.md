# 🏪 retail-pet-food-store-de

### 📌 Problem Statement:

A retail packaged food company operates stores at multiple locations across India. As the company grows, it needs to track key KPIs to better understand its business:

- Find out KPIs per store, per day, and per month
- Find out top-selling products per store across various geographic regions
- Find out least-selling products per store across various geographic regions

### 🎯 Solution Goal:

Stores drop their CRM data (orders, customers, products, store info) as flat csv files into a shared AWS S3 Data Lake. The pipeline:

- Reads the raw files from S3 using Apache Spark
- Cleans, transforms, and process data from layering stages (landing -> staging -> data_mart)
- Loads the processed data into a Delta Lake-based Data Warehouse using snowflake dimensional modelling
- Exposes the data via AWS Athena for HQ analytics and reporting

## 🏗️ Architecture:

<img width="1045" height="487" alt="image" src="https://github.com/user-attachments/assets/a570be22-6604-4b03-8998-64c311441df7" />

### Data Flow

Drop their data as flat csv files into a shared AWS S3 Data Lake landing stage -> Create databases and tables using Jupyter notebook -> Extract the data from landing stage csv files and clean the data -> push the clean and transformed data to staging stage -> Create snowflake data models and push into the AWS warehouse -> Creating HQ reporting.

## 🛠️ Tech Stack

| Technology  | Purpose |
| :---  | :---  |
| Python / PySpark  | Data processing and transformation  |
| Apache Spark  | Distributed computing engine  |
| Delta Lake  | ACID transactions on data lake  |
| AWS S3  | Cloud object storage  |
| AWS Athena  | Serverless SQL analytics  |
| Jupyter Notebooks  | Interactive development and Pipeline execution  |

## 📁 Project Structure

```
retail-pet-food-store-de
├── .gitignore
└── dw-with-pyspark/
    │
    ├── conf/
    │   ├── hive-site.xml                      # Hive metastore configuration
    │   └── spark-defaults.conf                # Spark session defaults
    │
    ├── docker-images/
    │   ├── pyspark-jupyter-lab/               # docker-compose.yml spark and jupyter configuration
    │   └── pyspark-cluster-with-jupyter/      # docker-compose.yml master-worker configuration
    |
    ├── datasets/
    │   ├── Customer/                          # Raw customer data files
    │   ├── Orders/                            # Raw orders data files
    │   ├── Product/                           # Raw product data files
    │   └── Store/                             # Raw store data files
    │
    ├── lib/
    │   ├── aws_s3.py                          # AWS S3 utility functions
    │   ├── job_control.py                     # Job control and orchestration helpers
    │   ├── spark_session.py                   # Spark session factory
    │   └── utils.py                           # General utility functions
    │
    ├── sql/
    │   └── athena.txt                         # Athena DDL/SQL scripts
    │
    ├── 01_init_db.ipynb                       # Initialize the database/schema
    ├── 02_date_landing.ipynb                  # Date dimension – landing layer
    ├── 03_date_staging.ipynb                  # Date dimension – staging layer
    ├── 04_date_dim.ipynb                      # Date dimension – final load
    ├── 05_store_landing.ipynb                 # Store dimension – landing layer
    ├── 06_store_staging.ipynb                 # Store dimension – staging layer
    ├── 07_store_dim.ipynb                     # Store dimension – final load
    ├── 08_customer_landing.ipynb              # Customer dimension – landing layer
    ├── 09_customer_staging.ipynb              # Customer dimension – staging layer
    ├── 10_customer_dim.ipynb                  # Customer dimension – final load (SCD2)
    ├── 11_product_landing.ipynb               # Product dimension – landing layer
    ├── 12_product_staging.ipynb               # Product dimension – staging layer
    ├── 13_product_dim.ipynb                   # Product dimension – final load (SCD2)
    ├── 14_sales_landing.ipynb                 # Sales fact – landing layer
    ├── 15_sales_staging.ipynb                 # Sales fact – staging layer
    ├── 16_sales_fact.ipynb                    # Sales fact – final load
    ├── 17_plan_type_dim.ipynb                 # Plan type dimension load
    ├── 98_generate_athena_ddl.ipynb           # Generate Athena DDL from Delta tables
    ├── 99_reset_warehouse.ipynb               # Reset/clean the warehouse (dev use)
    └── run_config.txt                         # rundate inital start date json config
```

## 🚀 Getting Started

### Prerequisites

- Python 3.3+
- Apache Spark 3.x
- AWS Account
- Delta Lake library (delta-spark)
- AWS credentials configured (~/.aws/credentials)
- Access to an AWS S3 bucket

### Configuration

1. Update /spark/conf/spark-defaults.conf with Spark and Delta Lake path.
2. Update /spark/conf/hive-site.xml with Hive metastore.
3. Update /spark/conf/spark-env.sh with aws access keys.

### Installation

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   ```

2. **Create AWS account**
3. **Create AWS user, AWS access keys, AWS s3 bucket structure**
4. **Install Docker desktop**
5. **Run Docker commands for both the docker-compose.yml files**
   ```bash
   docker compose up
   ```
6. **Run Docker images to create container from docker-desktop**
7. **Get the jupyter notebook token and create the password for jupyter notebook from localhost (docker container)**
8. **Set Up /spark/conf and ~/.aws/credentials files in jupyter notebook**

## 🔐 Security & Best Practices

1. **Credentials Management**
   - Create AWS access credentials
   - Implement role-based access control (RBAC) in AWS user permissions
     
## 📝 License

This project is part of a data engineering portfolio demonstration.

## 👤 Author

**Project**: Retail Pet Food Store AWS Data Engineering Pipeline  
**Technologies**: Apache Spark, Delta Lake, AWS S3, Python, AWS Athena, Jupyter Notebooks  
**Architecture**: Medallion (landing → staging → db)
