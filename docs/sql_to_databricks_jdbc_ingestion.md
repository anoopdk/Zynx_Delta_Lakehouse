# PostgreSQL to Databricks Ingestion Using JDBC

## Overview

Databricks supports direct JDBC ingestion from PostgreSQL into Bronze
Delta tables using two primary patterns:

-   **Incremental Load using `updated_at`**
-   **Snapshot Load using Row Hash Difference**

This approach is fully supported and production-ready without requiring
Azure Data Factory (ADF) in most scenarios.

------------------------------------------------------------------------

## Pattern A --- Incremental Ingestion (`updated_at`)

### When to Use

-   Source table has an `updated_at` column
-   `updated_at` is indexed
-   Stable primary key exists

### SQL Pattern

``` sql
SELECT *
FROM orders
WHERE updated_at > :last_watermark
```

### Databricks JDBC Example

``` python
df = spark.read.jdbc(
    url=jdbc_url,
    table="""(SELECT * FROM public.orders WHERE updated_at > TIMESTAMP '2025-01-01 00:00:00') t""",
    column="id",
    lowerBound=1,
    upperBound=100000000,
    numPartitions=16,
    properties=jdbc_props
)
```

### Benefits

-   True incremental loads
-   Low source load
-   Fast performance
-   Ideal for SCD2

------------------------------------------------------------------------

## Pattern B --- Snapshot + Row Hash Diff

### When to Use

-   No `updated_at` column
-   No CDC enabled

### Steps

#### 1. Full Snapshot Load

``` python
df = spark.read.jdbc(url=jdbc_url, table="public.countries", properties=jdbc_props)
```

#### 2. Row Hash Creation

``` python
df = df.withColumn("row_hash", sha2(concat_ws("||", *df.columns), 256))
```

#### 3. Diff Against Bronze

``` sql
SELECT s.*
FROM snapshot s
LEFT JOIN bronze b
  ON s.id = b.id
WHERE b.row_hash IS NULL
   OR s.row_hash <> b.row_hash
```

#### 4. Apply SCD2 Merge

------------------------------------------------------------------------

## Performance Considerations

### Expected Performance for 500k Rows

With proper tuning: **2--5 minutes**

### Required Optimizations

-   Numeric partition column
-   Index on PK
-   `numPartitions`
-   `fetchsize`
-   Proper cluster sizing

``` python
df = spark.read.jdbc(
    url=jdbc_url,
    table="public.countries",
    column="id",
    lowerBound=1,
    upperBound=500000,
    numPartitions=16,
    properties={
        "user": user,
        "password": password,
        "driver": "org.postgresql.Driver",
        "fetchsize": "10000"
    }
)
```

------------------------------------------------------------------------

## Do You Need ADF for PostgreSQL?

### You Do NOT Need ADF If:

-   Databricks has direct DB access
-   Secrets are stored securely
-   Firewall allows TCP 5432

### You DO Need ADF If:

-   Source is on-prem only
-   No PrivateLink support
-   Heavy OLTP workload
-   Audit and throttling required

------------------------------------------------------------------------

## Design Validation

✅ PostgreSQL → JDBC Incremental → Bronze Delta\
✅ PostgreSQL → Snapshot + Row Hash → Bronze Delta

Both are enterprise-grade ingestion patterns.

------------------------------------------------------------------------

## GDPR Considerations

If storing history (SCD2): - Implement Right-to-Erasure jobs - Mask or
encrypt PII - Propagate deletes

------------------------------------------------------------------------

## Final Source Compatibility Matrix

  Source       Direct JDBC?   Needs ADF?
  ------------ -------------- ------------
  PostgreSQL   Yes            No
  Azure SQL    Yes            No
  SQL Server   Yes            No
  ADLS         Native         No
  SFTP         No             Yes

------------------------------------------------------------------------

## Final Statement

Direct JDBC ingestion from PostgreSQL into Databricks Bronze using
either **incremental** or **snapshot + diff** is fully supported,
scalable, cost-effective, and compatible with Unity Catalog, Delta Lake,
and Declarative Pipelines.
