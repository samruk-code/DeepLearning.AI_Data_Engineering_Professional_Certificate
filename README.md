# DeepLearning.AI Data Engineering Professional Certificate

Labs and assignments from the [DeepLearning.AI Data Engineering Professional Certificate](https://www.deeplearning.ai/courses/data-engineering/) — a 4-course program taught by **Joe Reis**, co-author of *Fundamentals of Data Engineering*. All hands-on work runs on **AWS**, with labs built in partnership with AWS and Factored.AI.

---

## Table of Contents

- [Skills Demonstrated](#skills-demonstrated)
- [Capstone Thread — Product Recommendation System](#capstone-thread--product-recommendation-system)
- [Course 1 — Introduction to Data Engineering](#course-1--introduction-to-data-engineering)
  - [Lab 1 · End-to-End Batch Pipeline on AWS](#lab-1--end-to-end-batch-pipeline-on-aws)
  - [Lab 2 · Good Data Architecture — Security, Reliability & Scalability](#lab-2--good-data-architecture--security-reliability--scalability)
  - [Lab 3 · End-to-End Batch + Streaming Pipeline for a Recommendation System](#lab-3--end-to-end-batch--streaming-pipeline-for-a-recommendation-system)
- [Course 2 — Source Systems, Data Ingestion, and Pipelines](#course-2--source-systems-data-ingestion-and-pipelines)
  - [Week 1 — Source Systems & Database Connectivity](#week-1--source-systems--database-connectivity)
  - [Week 2 — Batch & Streaming Ingestion](#week-2--batch--streaming-ingestion)
  - [Week 3 — DataOps: Infrastructure as Code & Data Quality](#week-3--dataops-infrastructure-as-code--data-quality)
  - [Week 4 — Pipeline Orchestration with Apache Airflow](#week-4--pipeline-orchestration-with-apache-airflow)
- [Repository Structure](#repository-structure)
- [Certificate](#certificate)

---

## Skills Demonstrated

| Domain | Technologies |
|--------|-------------|
| Cloud Infrastructure | AWS RDS, EC2, S3, VPC, IAM, CloudFormation, Auto Scaling, ALB |
| Data Ingestion | Amazon Kinesis Data Streams, Kinesis Firehose, DynamoDB, REST APIs |
| Data Transformation | AWS Glue (PySpark ETL), AWS Lambda, Star Schema modeling |
| Pipeline Orchestration | Apache Airflow (TaskFlow API, dynamic DAGs, BranchPythonOperator) |
| Infrastructure as Code | Terraform (modules, state backends, outputs, variables) |
| Data Quality | Great Expectations (suites, validators, checkpoints, S3-backed stores) |
| Serving & Analytics | Amazon Athena, pgvector (PostgreSQL), Jupyter dashboards |
| Observability | Amazon CloudWatch, Apache Benchmark load testing |
| Programming | Python, SQL, boto3, PySpark |

---

## Capstone Thread — Product Recommendation System

A product recommendation system serves as the thread across both courses, growing progressively from a simple ETL into a full real-time inference pipeline:

```
Course 1, Lab 1   →  MySQL RDS → Glue ETL → S3 (star schema, Parquet)
Course 1, Lab 3   →  + Glue ETL for ML training data
                     + pgvector DB for embeddings
                     + Kinesis Streams → Lambda inference → Firehose → S3
Course 2, Week 2  →  + Streaming ETL consumer with enrichment & routing
Course 2, Week 4  →  + Airflow ML pipeline with data quality gates & branch logic
```

---

## Course 1 — Introduction to Data Engineering

### Lab 1 · End-to-End Batch Pipeline on AWS
**[Introduction to Data Engineering/Lab_1/](Introduction%20to%20Data%20Engineering/Lab_1/)**

Built a complete data engineering lifecycle on AWS for a retail use case. Provisioned all infrastructure with **Terraform**, ran an **AWS Glue ETL job** that extracted data from a MySQL **Amazon RDS** instance, transformed the normalized OLTP schema into a **star schema** (Parquet format), and loaded it to **Amazon S3**. Queried the result with **Amazon Athena** and built a sales analytics dashboard in **Jupyter Notebook**.

`Terraform` `AWS Glue` `RDS MySQL` `S3` `Athena` `Star Schema` `Parquet` `IaC`

---

### Lab 2 · Good Data Architecture — Security, Reliability & Scalability
**[Introduction to Data Engineering/Lab_2/](Introduction%20to%20Data%20Engineering/Lab_2/)**

Assessed and hardened a three-tier web application on AWS using the **AWS Well-Architected Framework** as a guide. Tasks included:

- **Security**: Configured VPC security groups to restrict ALB inbound traffic to port 80 only, closing an exposed private data endpoint on port 90.
- **Reliability**: Verified multi-AZ EC2 deployment via Application Load Balancer — traffic distributed across Availability Zones for fault tolerance.
- **Scalability**: Created a target-tracking Auto Scaling policy (ALB request count metric), right-sized instances from `t3.micro` → `t3.nano`, and validated scale-out/in under load using **Apache Benchmark** stress tests monitored with **Amazon CloudWatch**.

`VPC` `Security Groups` `EC2 Auto Scaling` `ALB` `CloudWatch` `AWS Well-Architected Framework`

---

### Lab 3 · End-to-End Batch + Streaming Pipeline for a Recommendation System
**[Introduction to Data Engineering/Lab_3/](Introduction%20to%20Data%20Engineering/Lab_3/)**

Translated ML team requirements into a full production-style pipeline across two architectures:

**Batch pipeline**: A Terraform-provisioned AWS Glue ETL job ingested product/user ratings from RDS MySQL, transformed and partitioned the data by customer into an S3 data lake for ML training.

**Streaming pipeline**: Created a PostgreSQL RDS database with the **pgvector** extension to store item and user embeddings. Wired up an **AWS Lambda** inference function to consume real-time user activity events from **Kinesis Data Streams**, compute product recommendations using a pre-trained model, and deliver results to S3 via **Kinesis Firehose**.

`Terraform` `AWS Glue` `Kinesis Data Streams` `Kinesis Firehose` `Lambda` `pgvector` `Vector Databases` `ML Pipelines`

---

## Course 2 — Source Systems, Data Ingestion, and Pipelines

### Week 1 — Source Systems & Database Connectivity
**[Source Systems, Data Ingestion, and Pipelines/W1/](Source%20Systems%2C%20Data%20Ingestion%2C%20and%20Pipelines/W1/)**

- Queried relational data with SQL against Amazon RDS (Lab 1)
- Explored NoSQL patterns with **Amazon DynamoDB** (Lab 2)
- Worked with S3 object storage and the AWS CLI/boto3 (Lab 3)
- **Graded Assignment**: Diagnosed and resolved EC2-to-RDS connectivity failures — identified misconfigured security groups and IAM permission errors, then inserted and queried data via **psql** from a bastion host.

`RDS` `DynamoDB` `S3` `VPC` `Security Groups` `IAM` `Bastion Host` `Connectivity Troubleshooting`

---

### Week 2 — Batch & Streaming Ingestion
**[Source Systems, Data Ingestion, and Pipelines/W2/](Source%20Systems%2C%20Data%20Ingestion%2C%20and%20Pipelines/W2/)**

**Streaming Lab**: Implemented a streaming ETL consumer using **Amazon Kinesis Data Streams** and **boto3**. The consumer ingested e-commerce browsing events, enriched the records with processing timestamps, cart metrics, and product counts, routed them to USA or International Kinesis streams, and delivered the transformed data to S3 via **Kinesis Firehose**.

**Graded Assignment (Spotify API Batch Pipeline)**: Built a batch ingestion pipeline against the Spotify Web API. Implemented OAuth2 token authentication, handled **API pagination** (offset/cursor patterns), built automatic token-refresh logic for long-running jobs, and extracted paginated album track data to JSON. All API interactions were coded in Python using the `requests` library.

`Kinesis Data Streams` `Kinesis Firehose` `boto3` `REST APIs` `OAuth2` `Pagination` `Batch Ingestion` `Streaming ETL`

---

### Week 3 — DataOps: Infrastructure as Code & Data Quality
**[Source Systems, Data Ingestion, and Pipelines/W3/](Source%20Systems%2C%20Data%20Ingestion%2C%20and%20Pipelines/W3/)**

**Terraform Lab**: Wrote Terraform configuration from scratch to deploy a private RDS PostgreSQL instance inside a VPC with a public EC2 bastion host, including networking (subnets, route tables, security groups), remote state management, and teardown.

**CloudWatch Lab**: Configured monitoring dashboards and metrics for data pipeline resources.

**Graded Assignment (Great Expectations)**: Implemented a production-grade data quality workflow:
- Configured **File Data Context** backed by S3 (expectations store, validations store, checkpoint store)
- Connected to MySQL RDS as a GX Data Source; created a Table Data Asset split into batches by `vendor_id`
- Defined Expectation Suites (null checks, value range assertions)
- Built a **Checkpoint** that validated all batches and published human-readable **Data Docs** to a static S3 website
- Verified the pipeline caught an injected bad record violating a range expectation.

`Terraform` `Great Expectations` `CloudWatch` `IaC` `Data Quality` `S3` `VPC` `Bastion Host`

---

### Week 4 — Pipeline Orchestration with Apache Airflow
**[Source Systems, Data Ingestion, and Pipelines/W4/](Source%20Systems%2C%20Data%20Ingestion%2C%20and%20Pipelines/W4/)**

**Airflow 101 Lab**: Authored and operated DAGs in the Airflow UI — triggered runs, monitored task state, cleared and retried failed tasks.

**Best Practices Lab**: Refactored DAGs using task grouping and Airflow Connections for database credentials.

**Graded Assignment (Advanced ML Pipeline)**: Built a multi-vendor ML pipeline for ride-duration prediction using the **TaskFlow API**:
- `data_quality` task — ran **Great Expectations** validations inside a virtualenv decorator
- `train_and_evaluate` task — trained and scored a model on preprocessed Parquet data
- `is_deployable` task — **BranchPythonOperator** routing: deploy or notify based on model metrics
- Generalized the single DAG into a **Jinja-templated DAG factory** that generates one DAG per vendor config file (Alitran, Easy Destiny, ToMyPlaceAI) at import time.

`Apache Airflow` `TaskFlow API` `BranchPythonOperator` `Great Expectations` `Dynamic DAGs` `Parquet` `ML Pipeline Orchestration`

---

## Repository Structure

```
.
├── Introduction to Data Engineering/
│   ├── Lab_1/          # ETL pipeline: RDS → Glue → S3 → Athena
│   ├── Lab_2/          # Architecture assessment: security, scaling, monitoring
│   └── Lab_3/          # Batch + streaming recommendation pipeline
│
└── Source Systems, Data Ingestion, and Pipelines/
    ├── W1/             # Source systems: SQL, DynamoDB, S3, connectivity troubleshooting
    ├── W2/             # Kinesis streaming ETL + Spotify API batch ingestion
    ├── W3/             # Terraform IaC + Great Expectations data quality
    └── W4/             # Apache Airflow: TaskFlow API + dynamic DAGs
```

---

## Certificate

[📄 View Certificate (PDF)](Data%20Engineering%20Professional%20Certificate.pdf)

Issued by **DeepLearning.AI** · Instructor: **Joe Reis**
