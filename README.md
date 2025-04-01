# Snowflake S3 Integration Project 🌩️💻

## 📚 Project Overview
This project demonstrates the integration of **Snowflake** with **AWS S3** to load data from a CSV file stored in an S3 bucket into a Snowflake table. The goal is to streamline the process of data ingestion and transformation using cloud technologies.

## 🔧 Technologies Used
- **Snowflake**: Cloud data platform for data warehousing.
- **AWS S3**: Cloud storage service for storing data.
- **SQL**: To interact with Snowflake and perform data loading operations.

## ⚡ Project Structure
```
snowflake-s3-integration/
│── scripts/                # SQL scripts for Snowflake operations
│   ├── create_stage.sql    # Script to create Snowflake stage for S3 integration
│   ├── create_table.sql    # Script to create table in Snowflake
│   ├── copy_into.sql       # Script to load data from S3 into Snowflake
│── config/                 # Configuration files (e.g., credentials, integrations)
│   ├── aws_credentials.json   # AWS credentials (Never push to Git!)
│   ├── snowflake_config.json  # Snowflake config (Never push to Git!)
│── data/                   # Sample data files (ignored in Git)
│── .gitignore              # Git ignore rules
│── README.md               # Project documentation
```

## 🛠️ Setup Instructions

### 1. **Configure AWS S3 and Snowflake**
- **AWS S3**: Ensure you have a bucket where your CSV file (`customer_info.csv`) is stored.
- **Snowflake**: Create a Snowflake account and set up an integration for S3 storage. Follow Snowflake documentation for configuring storage integrations.

### 2. **Set Up AWS Integration**
Run the following command in Snowflake to create a storage integration:
```sql
CREATE OR REPLACE STORAGE INTEGRATION aws_s3_integration
   TYPE = EXTERNAL_STAGE
   STORAGE_PROVIDER = 'S3'
   STORAGE_AWS_ROLE_ARN = 'your-aws-role-arn'
   ENABLED = TRUE;
```

### 3. **Run SQL Scripts**

#### Create Stage:
Run the `create_stage.sql` script to create a Snowflake stage pointing to your S3 bucket.
```sql
CREATE OR REPLACE STAGE AWS_DEMO_STAGE
STORAGE_INTEGRATION = aws_s3_integration
URL = 's3://your-bucket-name/'
FILE_FORMAT = (TYPE = 'CSV' SKIP_HEADER = 1 FIELD_OPTIONALLY_ENCLOSED_BY = '"');
```

#### Create Table:
Run the `create_table.sql` script to create the `demo_customer_info` table in Snowflake.
```sql
CREATE OR REPLACE TEMPORARY TABLE demo_customer_info (
    id INTEGER,
    first_name STRING,
    last_name STRING,
    company STRING,
    email STRING,
    workphone STRING,
    cellphone STRING,
    streetaddress STRING,
    city STRING,
    postalcode STRING
);
```

#### Load Data:
Use the `copy_into.sql` script to load data from your S3 bucket into Snowflake.
```sql
COPY INTO demo_customer_info
FROM @AWS_DEMO_STAGE/customer_info.csv
FILE_FORMAT = (TYPE = 'CSV' SKIP_HEADER = 1 FIELD_OPTIONALLY_ENCLOSED_BY = '"');
```

### 4. **Verify Data**
You can run the following SQL to verify the data is loaded into Snowflake:
```sql
SELECT * FROM demo_customer_info LIMIT 10;
```

## 🌟 .gitignore
Make sure you add sensitive files like credentials and configuration files to `.gitignore`:
```bash
# Ignore credentials and secrets
*.csv
*.json
*.pem
.env
```
