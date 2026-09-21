# Data Quality

## 1. Overview

Data quality checks are applied during the Silver-layer transformation to ensure that raw API data is converted into clean and trustworthy datasets before reaching the Gold layer.

The objective is to prevent invalid, incomplete, or duplicate records from being used for downstream analytics.

---

## 2. Data Quality Flow

```text
Bronze
match_raw
   ↓
PySpark Transformation
   ↓
Data Quality Checks
   ↓
Clean Silver Data
   ↓
Gold
3. Null Validation

Critical fields are checked for missing values.

Examples include:

match_id
batting_team
runs
overs

Records failing critical validation are identified before downstream processing.

4. Range Validation

Domain-specific range checks are applied to identify physically invalid cricket values.

Examples include:

Field	Validation
Runs	Valid T20 range
Wickets	Valid T20 range
Overs	Valid T20 range

These checks help prevent invalid source values from entering the analytics layer.

5. Duplicate Validation

Duplicate records are checked using the business-level uniqueness rule.

For innings data:

match_id + innings_order

should uniquely identify an innings record.

Duplicate records are identified during the Silver transformation.

6. Structural Validation

The API response contains nested cricket data.

The Silver transformation:

Flattens nested structures
Explodes innings arrays
Extracts required attributes
Converts the raw structure into tabular data

This ensures that the downstream dataset has a consistent structure.

7. Data Cleaning

The Silver layer is responsible for converting raw API data into clean and structured datasets.

The transformation process includes:

Raw Nested Data
      ↓
Flatten
      ↓
Explode
      ↓
Validate
      ↓
Clean Silver Dataset
8. Invalid Record Handling

Invalid records should not silently enter downstream analytics.

Records failing validation are identified and handled during the Silver transformation process.

This prevents data quality issues from propagating into the Gold layer and reporting.

9. Quality Checks Before Gold

Before Gold-layer processing, the Silver datasets should satisfy the required:

Null checks
Range checks
Duplicate checks
Structural checks
Required-field validation

Only validated Silver data is passed to the Gold transformation.

10. Why Data Quality Matters

Data quality checks help ensure that:

Analytics are based on valid data
Duplicate records do not inflate metrics
Missing critical values are identified
Invalid cricket statistics are detected
Downstream Power BI reporting is more reliable
11. Technologies
PySpark
Microsoft Fabric
Fabric Lakehouse
Delta Lake
Python
Data Validation
Data Quality Checks
