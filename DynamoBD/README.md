# Amazon DynamoDB Lab: Creating a Students Table on AWS

A hands-on lab where I created a NoSQL table in **Amazon DynamoDB**, added items to it, and queried them using the AWS Management Console.

![Platform](https://img.shields.io/badge/AWS-DynamoDB-4053D6?logo=amazondynamodb&logoColor=white)
![Region](https://img.shields.io/badge/Region-us--east--1-orange)
![Type](https://img.shields.io/badge/Database-NoSQL-green)

---

## Table of Contents

- [What is Amazon DynamoDB?](#what-is-amazon-dynamodb)
- [Key Concepts](#key-concepts)
- [Benefits](#benefits)
- [Common Use Cases](#common-use-cases)
- [Lab Overview](#lab-overview)
- [Step-by-Step Guide](#step-by-step-guide)
- [Screenshots](#screenshots)
- [Optional: Same Lab with AWS CLI](#optional-same-lab-with-aws-cli)
- [Clean Up](#clean-up)
- [What I Learned](#what-i-learned)
- [Author](#author)

---

## What is Amazon DynamoDB?

**Amazon DynamoDB** is a fully managed, serverless **NoSQL database** service from AWS. It stores data as key-value and document items and delivers single-digit millisecond performance at any scale. There are no servers to provision, patch, or manage. AWS handles replication, scaling, backups, and availability for you.

Unlike a relational database (MySQL, PostgreSQL), DynamoDB does not require a fixed schema. Only the primary key is defined up front. Every other attribute can differ from item to item.

---

## Key Concepts

| Concept | Meaning |
|---|---|
| **Table** | A collection of data, similar to a table in a relational database |
| **Item** | A single record in a table (like a row) |
| **Attribute** | A piece of data inside an item (like a column), e.g. `Name`, `Age` |
| **Partition key** | The primary key that uniquely identifies each item and decides where it is stored |
| **Sort key (optional)** | A second key that allows multiple items to share a partition key |
| **Capacity mode** | How you pay for reads and writes: *On-demand* or *Provisioned* |
| **Scan / Query** | Ways to read data. A Scan reads the whole table, a Query reads by key |

---

## Benefits

- **Fully managed and serverless**: no servers, patching, or maintenance
- **Fast and scalable**: consistent single-digit millisecond latency, even at huge scale
- **Flexible schema**: items in the same table can have different attributes
- **Pay for what you use**: on-demand mode charges per request
- **Highly available**: data is automatically replicated across multiple Availability Zones
- **Secure**: encryption at rest, IAM-based access control, and VPC endpoints
- **Built-in features**: backups, point-in-time recovery, TTL, streams, and global tables
- **Easy integration**: works with Lambda, API Gateway, S3, and other AWS services

---

## Common Use Cases

- Web and mobile app backends (user profiles, sessions)
- Gaming (leaderboards, player state)
- Shopping carts and e-commerce catalogs
- IoT and time-series data
- Serverless applications with AWS Lambda
- Student, employee, or inventory record systems (like this lab)

---

## Lab Overview

| Setting | Value |
|---|---|
| Service | Amazon DynamoDB |
| Region | US East (N. Virginia), `us-east-1` |
| Table name | `Students` |
| Partition key | `StudentID` (String) |
| Sort key | None |
| Capacity mode | On-demand |
| Deletion protection | Off |

### Table structure

| Attribute | Type | Example |
|---|---|---|
| `StudentID` (partition key) | String | `21674` |
| `Age` | Number | `24` |
| `Course` | String | `AWS Cloud Security` |
| `Name` | String | `Sufyan Ali` |

### Items created in the lab

| StudentID | Name | Age |
|---|---|---|
| 1234 | Saad | 20 |
| 1411 | Ali | 22 |
| 21674 | Sufyan Ali | 24 |

---

## Step-by-Step Guide

### Step 1: Create the table

1. Sign in to the **AWS Management Console** and open **DynamoDB**.
2. Make sure your region is **US East (N. Virginia)**.
3. Click **Tables** → **Create table**.
4. Enter the table name: `Students`.
5. Enter the partition key: `StudentID`, type **String**.
6. Leave the default settings (on-demand capacity) and click **Create table**.
7. Wait until the status shows **Active**.

### Step 2: Add items

1. Open the `Students` table and click **Explore items** → **Create item**.
2. `StudentID` is already listed as the partition key. Enter its value.
3. Click **Add new attribute** and add:
   - `Age` (Number)
   - `Course` (String)
   - `Name` (String)
4. Click **Create item** (or **Save and close** when editing).
5. Repeat for the remaining students.

### Step 3: Read the data

1. Go to **Explore items** and select the `Students` table.
2. Click **Run** to scan the table.
3. All items are returned in the results list.

---

## Screenshots

### 1. Table created successfully
The `Students` table is **Active**, with `StudentID (S)` as the partition key and on-demand capacity.

![Table created](images/01-table-created.png)

### 2. Adding / editing an item
An item with `StudentID`, `Age`, `Course`, and `Name` attributes.

![Edit item](images/02-edit-item.png)

### 3. Scanning the table
The scan returns all 3 student items.

![Table items](images/03-items-returned.png)

---

## Optional: Same Lab with AWS CLI

Create the table:

```bash
aws dynamodb create-table \
  --table-name Students \
  --attribute-definitions AttributeName=StudentID,AttributeType=S \
  --key-schema AttributeName=StudentID,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

Add an item:

```bash
aws dynamodb put-item \
  --table-name Students \
  --item '{
    "StudentID": {"S": "21674"},
    "Name": {"S": "Sufyan Ali"},
    "Age": {"N": "24"},
    "Course": {"S": "AWS Cloud Security"}
  }' \
  --region us-east-1
```

Read an item by key:

```bash
aws dynamodb get-item \
  --table-name Students \
  --key '{"StudentID": {"S": "21674"}}' \
  --region us-east-1
```

Scan the whole table:

```bash
aws dynamodb scan --table-name Students --region us-east-1
```

---

## Clean Up

To avoid unexpected charges, delete the table when you are done:

1. Open **DynamoDB** → **Tables**.
2. Select `Students` → **Delete**.
3. Confirm the deletion.

Or with the CLI:

```bash
aws dynamodb delete-table --table-name Students --region us-east-1
```

---

## What I Learned

- How NoSQL databases differ from relational databases
- How to choose and define a partition key
- How to add items with flexible attributes
- The difference between a Scan and a Query
- How on-demand capacity removes the need to plan throughput
- How to do the same tasks from the console and the AWS CLI

---

## Author

**Sufyan Ali**
Cloud Computing / AWS Cloud Security student

- GitHub: [your-username](https://github.com/your-username)
- LinkedIn: [your-profile](https://www.linkedin.com/in/your-profile)

---

*If you found this helpful, give the repo a star.*
