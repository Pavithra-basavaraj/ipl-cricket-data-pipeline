# IPL Cricket Data Engineering Pipeline — Architecture

## 1. Project Overview

This project builds an end-to-end data engineering pipeline for IPL cricket match data.

The pipeline replaces static datasets with API-driven data ingestion and transforms raw cricket data into analytics-ready datasets for reporting and performance analysis.

## 2. Business Objective

The objective is to build a reliable and scalable data pipeline that can:

- Ingest IPL data from a REST API
- Handle API failures and retries
- Store raw source data
- Validate data quality
- Transform data using PySpark
- Maintain historical data
- Build analytics-ready datasets
- Support cricket performance analysis through Power BI

## 3. Technology Stack

- Python
- RapidAPI
- Microsoft Fabric
- PySpark
- MySQL
- Power BI
- Git / GitHub

## 4. High-Level Architecture

```text
                 RapidAPI
                    │
                    ▼
            Python API Ingestion
                    │
                    ▼
          Retry / Exponential Backoff
                    │
                    ▼
              Bronze Layer
                    │
                    ▼
          Data Quality Validation
                    │
                    ▼
             PySpark Transformations
                    │
                    ▼
              Silver Layer
                    │
                    ▼
             Data Modeling
                    │
                    ▼
              Star Schema
                    │
                    ▼
              SCD Type 2
                    │
                    ▼
               Gold Layer
                    │
             ┌──────┴──────┐
             ▼             ▼
           MySQL        Power BI
                           │
                           ▼
                       DAX / KPIs
                           │
                           ▼
                       Dashboard
