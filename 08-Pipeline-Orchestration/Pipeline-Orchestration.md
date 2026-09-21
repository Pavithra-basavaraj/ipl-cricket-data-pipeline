```

around several sections.

Use the clean version below **exactly as the content of `Pipeline-Orchestration.md`**:

```markdown
# Pipeline Orchestration

## 1. Overview

The IPL Cricket Data Engineering Pipeline is orchestrated using a Microsoft Fabric Data Pipeline.

The pipeline coordinates API ingestion, Silver transformations, and Gold data modeling in a controlled execution flow.

The orchestration ensures that downstream processing starts only after the required upstream activity completes successfully.

---

## 2. Pipeline Architecture

The Fabric pipeline follows this execution flow:

```text
LookUp_Config
      ↓
   Filter1
      ↓
   ForEach1
      ↓
 NB_API_Ingestion
      ↓
nb_silver_matches
      ↓
nb_gold_star_schema
```

---

## 3. Pipeline Activities

### 3.1 LookUp_Config

The `LookUp_Config` activity retrieves the configuration required for the pipeline execution.

The configuration is passed to the next stage of the pipeline for further processing.

---

### 3.2 Filter1

The `Filter1` activity filters the configuration records based on the processing requirements.

Only the required configuration records are passed to the `ForEach1` activity.

---

### 3.3 ForEach1

The `ForEach1` activity iterates through the filtered configuration records.

The API ingestion notebook is executed within the ForEach activity.

```text
ForEach1
   └── NB_API_Ingestion
```

This allows the ingestion process to be executed for each required configuration item.

---

### 3.4 NB_API_Ingestion

The `NB_API_Ingestion` notebook performs the Bronze-layer API ingestion.

It:

- Connects to the cricket API
- Retrieves match data
- Handles API ingestion logic
- Stores raw data in the Fabric Lakehouse
- Writes the ingested data to the Bronze table

Bronze output:

```text
match_raw
```

---

### 3.5 nb_silver_matches

The `nb_silver_matches` notebook processes the Bronze data using PySpark.

The notebook performs transformations required to convert the raw API data into structured Silver-layer datasets.

Silver outputs include:

```text
silver_innings
silver_match_innings
```

---

### 3.6 nb_gold_star_schema

The `nb_gold_star_schema` notebook transforms the Silver-layer data into analytics-ready Gold datasets.

This layer implements the dimensional data model used for downstream analytics and reporting.

The Gold layer contains dimension and fact tables based on the project's Star Schema design.

---

## 4. Activity Dependencies

The pipeline uses activity dependencies to control the execution order.

The processing follows:

```text
LookUp_Config
      ↓
   Filter1
      ↓
   ForEach1
      ↓
NB_API_Ingestion
      ↓
nb_silver_matches
      ↓
nb_gold_star_schema
```

The Silver notebook runs after the ingestion stage completes successfully.

The Gold notebook runs after the Silver transformation completes successfully.

This dependency-based execution helps prevent downstream processing from running before the required upstream data is available.

---

## 5. Pipeline Execution

A successful pipeline execution was validated in Microsoft Fabric.

### Latest successful run

| Activity | Status |
|---|---|
| LookUp_Config | Succeeded |
| Filter1 | Succeeded |
| ForEach1 | Succeeded |
| NB_API_Ingestion | Succeeded |
| NB_API_Ingestion | Succeeded |
| nb_silver_matches | Succeeded |
| nb_gold_star_schema | Succeeded |

The `NB_API_Ingestion` activity appears twice in the run history because it was executed for two items within the `ForEach1` activity.

### Pipeline Status

```text
Pipeline Status: Succeeded
```

The successful execution confirms that the configured orchestration completed across the ingestion, Silver transformation, and Gold modeling stages.

---

## 6. Execution Monitoring

Microsoft Fabric provides execution details for each pipeline activity, including:

- Activity status
- Run start time
- Execution duration
- Input
- Output
- Pipeline Run ID

This allows individual activities to be monitored and helps identify where a pipeline execution may fail.

---

## 7. Pipeline Benefits

The orchestration provides:

- Automated execution of multiple processing stages
- Dependency-based execution
- Separation of ingestion and transformation workloads
- Reusable notebook-based processing
- Centralized pipeline monitoring
- Easier troubleshooting of individual activities
- Controlled Bronze → Silver → Gold processing

---

## 8. Technologies

- Microsoft Fabric Data Pipeline
- Microsoft Fabric Lakehouse
- Python
- PySpark
- Delta Lake
- REST API
```

