# Snowflake CI/CD Example

This repository demonstrates CI/CD for Snowflake using GitHub Actions.

## Structure

```
├── .github/workflows/
│   └── deploy.yml          # GitHub Actions workflow
├── sql/
│   ├── 01_setup_database.sql    # Database, schema, and tables
│   ├── 02_stored_procedures.sql # ETL stored procedures
│   └── 03_tasks.sql             # Scheduled tasks
```

## Setup

### GitHub Secrets Required

Configure these secrets in your GitHub repository (Settings > Secrets > Actions):

| Secret | Description |
|--------|-------------|
| `SNOWFLAKE_ACCOUNT` | Your Snowflake account identifier |
| `SNOWFLAKE_USER` | Snowflake username |
| `SNOWFLAKE_PASSWORD` | Snowflake password |
| `SNOWFLAKE_WAREHOUSE` | Warehouse to use (e.g., COMPUTE_WH) |

## ETL Pipeline

The pipeline creates:

### Tables
- **RAW_DATA**: Landing table for incoming data
- **TRANSFORMED_DATA**: Processed/transformed records
- **AGGREGATED_DATA**: Daily aggregations

### Stored Procedures
- **EXTRACT_AND_TRANSFORM()**: Transforms raw data (uppercase + suffix)
- **AGGREGATE_DATA()**: Creates daily record counts
- **RUN_FULL_ETL_PIPELINE()**: Orchestrates the full ETL

### Tasks
- **TASK_EXTRACT_TRANSFORM**: Runs hourly (cron: `0 * * * *`)
- **TASK_AGGREGATE**: Runs after extract/transform completes

## Manual Execution

```sql
CALL CICD.ETL.RUN_FULL_ETL_PIPELINE();
```
