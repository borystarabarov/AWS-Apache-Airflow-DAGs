# AWS Apache Airflow Data Pipeline

This repository contains an Apache Airflow DAG that orchestrates a data processing pipeline using various AWS services.

## Architecture

The pipeline performs the following steps:
1. Monitors S3 bucket for new data files
2. Catalogs data using AWS Glue Crawler
3. Transforms raw data using AWS Glue job
4. Performs aggregations using EMR Serverless
5. Loads processed data back to S3

## Prerequisites

- Apache Airflow environment (MWAA recommended)
- AWS account with access to:
  - S3
  - Glue
  - EMR Serverless
- Required IAM roles and permissions

## Configuration

The following environment variables need to be configured:
- `S3_BUCKET_NAME`: Target S3 bucket for data and scripts
- `GLUE_ROLE_ARN`: IAM role ARN for Glue services
- `EXEC_ROLE_ARN`: IAM role ARN for MWAA execution

## DAG Structure

The DAG (`data_pipeline.py`) includes the following tasks:
1. `s3_sensor`: Monitors for new data files
2. `glue_crawler`: Catalogs raw data
3. `glue_job`: Transforms raw data
4. `create_emr_serverless_app`: Creates EMR Serverless application
5. `start_emr_serverless_job`: Runs aggregation job
6. `wait_for_emr_serverless_job`: Waits for job completion
7. `delete_app`: Cleans up EMR resources

## Setup Instructions

1. Clone this repository
2. Deploy the code to your Airflow environment
3. Place required Python scripts in the S3 bucket:
   - `scripts/glue/nyc_raw_to_transform.py`
   - `scripts/emr/nyc_aggregations.py`
4. Configure the necessary AWS credentials and connections in Airflow
5. Enable and trigger the DAG

## Schedule

The pipeline runs daily at 3:00 AM UTC.