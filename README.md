# AWS-Athena Service

# 🧠 AWS Athena Setup and SQL Query Project

### This project demonstrates how to use Amazon S3 + AWS Athena to query structured CSV data efficiently using both basic and advanced SQL.

## 🪣 Step 1: Create S3 Buckets

1.1 Data Bucket

### Create an S3 bucket to store your dataset:

<img width="1916" height="923" alt="Screenshot 2025-10-31 220928" src="https://github.com/user-attachments/assets/b1e96c1f-1210-4f62-8c2a-f15fe4964500" />

### Inside it, maintain this structure:

<img width="1919" height="902" alt="Screenshot 2025-10-31 221150" src="https://github.com/user-attachments/assets/2c9f422a-2fc6-440e-971f-0b448f9cb670" />

<img width="1919" height="923" alt="Screenshot 2025-10-31 221201" src="https://github.com/user-attachments/assets/b2a0da1e-0b69-4f4d-9258-09d947549b9d" />

<img width="1918" height="927" alt="Screenshot 2025-10-31 224832" src="https://github.com/user-attachments/assets/1dfc73e2-051d-4eda-9d99-44409ff24f42" />

s3://my-pec-bucket-1/orders/
    └── partitioned/
        ├── orders_delivered/
        ├── orders_pending/
        ├── orders_cancelled/
        └── orders_shipped/


### 1.2 Query Results Bucket

Athena needs a separate S3 bucket to store query results. Example:

<img width="1919" height="920" alt="Screenshot 2025-10-31 221055" src="https://github.com/user-attachments/assets/3269d353-38c9-42ab-bec8-c05fb4541be8" />


Go to Athena → Settings → Manage → Query result location and set this path.

<img width="1919" height="917" alt="Screenshot 2025-10-31 221408" src="https://github.com/user-attachments/assets/a29d50a9-5bcd-4117-aac2-ea0056c0e205" />

<img width="1919" height="924" alt="Screenshot 2025-10-31 221419" src="https://github.com/user-attachments/assets/c42b5fdc-2b70-44e8-9aaa-e9e29774cfe4" />

<img width="1919" height="918" alt="Screenshot 2025-10-31 221759" src="https://github.com/user-attachments/assets/04a63f98-fa37-42cf-80a8-819fc9abf4c3" />



## 🧱 Step 2: Create Athena Database

Run this query in the Athena Query Editor:

<img width="1919" height="918" alt="Screenshot 2025-10-31 221759" src="https://github.com/user-attachments/assets/c7fb17f7-6320-4cfb-96b9-53692e0a94ad" />

## 📋 Step 3: Create External Table

Create an external table linked to your partitioned dataset:

<img width="1917" height="908" alt="Screenshot 2025-10-31 222440" src="https://github.com/user-attachments/assets/b702b391-7d79-4dbd-a9fc-11667924161b" />

Load Partitions

<img width="1919" height="917" alt="Screenshot 2025-10-31 230306" src="https://github.com/user-attachments/assets/38585955-b4e0-46b2-9b99-0c989397ae3c" />


<img width="1918" height="923" alt="Screenshot 2025-10-31 225636" src="https://github.com/user-attachments/assets/071ce6fd-bf90-45e3-9c51-b705cb63f28d" />

## 🧮 Step 4: Basic SQL Queries

-- 1. Show all data
SELECT * FROM suyashdb.orders_partitioned LIMIT 10;

<img width="1914" height="934" alt="Screenshot 2025-10-31 223401" src="https://github.com/user-attachments/assets/d5207e36-0e51-48ba-afa9-25fd63daefce" />

<img width="1916" height="918" alt="Screenshot 2025-10-31 223409" src="https://github.com/user-attachments/assets/c3b165cf-361a-4417-b667-21f40497dbd7" />


-- 2. Count total orders
SELECT COUNT(*) AS total_orders FROM suyashdb.orders_partitioned;

<img width="1919" height="916" alt="Screenshot 2025-10-31 223536" src="https://github.com/user-attachments/assets/fc21b344-98e4-465c-b295-a62574932abd" />

<img width="1909" height="926" alt="Screenshot 2025-10-31 223543" src="https://github.com/user-attachments/assets/a5811669-6c1c-471b-bcfe-8b25bd39a297" />

-- 3. Filter by status
SELECT * FROM suyashdb.orders_partitioned WHERE status = 'delivered';

<img width="1914" height="881" alt="Screenshot 2025-10-31 223605" src="https://github.com/user-attachments/assets/470ac15d-763c-4a61-819b-6a434d1b9301" />

<img width="1919" height="921" alt="Screenshot 2025-10-31 223615" src="https://github.com/user-attachments/assets/85c7aab6-e4db-4f3b-b043-a314d9f92d42" />


-- 4. Total sales by status
SELECT status, SUM(total_amount) AS total_sales
FROM suyashdb.orders_partitioned
GROUP BY status;


## 🚀 Step 5: Advanced SQL Queries

-- 1. Total revenue by customer
SELECT customer_id, SUM(total_amount) AS total_spent
FROM suyashdb.orders_partitioned
GROUP BY customer_id
ORDER BY total_spent DESC;

<img width="1918" height="909" alt="Screenshot 2025-10-31 231249" src="https://github.com/user-attachments/assets/50303ef7-5b61-4dc0-a9dc-4c7e7ced0696" />

-- 2. Top 5 products by sales
SELECT product_id, SUM(quantity) AS total_sold
FROM suyashdb.orders_partitioned
GROUP BY product_id
ORDER BY total_sold DESC
LIMIT 5;

<img width="1919" height="918" alt="Screenshot 2025-10-31 231300" src="https://github.com/user-attachments/assets/36b73de1-e67a-4a7f-9fd2-c294c67a39e6" />


-- 3. Average order value per status
SELECT status, AVG(total_amount) AS avg_order_value
FROM suyashdb.orders_partitioned
GROUP BY status;

<img width="1919" height="913" alt="Screenshot 2025-10-31 231839" src="https://github.com/user-attachments/assets/4594619a-f663-4ab3-be80-c9c098eccb54" />


-- 4. Monthly sales trend
SELECT substr(order_date, 1, 7) AS month, SUM(total_amount) AS total_sales
FROM suyashdb.orders_partitioned
GROUP BY substr(order_date, 1, 7)
ORDER BY month;

-- 5. Customer order count and total spent
SELECT customer_id,
       COUNT(order_id) AS total_orders,
       SUM(total_amount) AS total_spent
FROM suyashdb.orders_partitioned
GROUP BY customer_id
ORDER BY total_spent DESC;

------------------
-----------------

## 🧹 Step 6: Cleanup (Optional)

To delete everything from Athena (metadata only — S3 data stays safe):

✅ You’ve now completed:

Proper S3 + Query Results setup

Athena DB and external table creation

Partition repair for optimized queries

Basic & advanced SQL operations on S3 data

# Author : Suyash Dahitule
