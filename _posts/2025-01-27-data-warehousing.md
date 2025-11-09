---
title: 'Data Warehousing'
date: 2025-01-27
permalink: /posts/datawarehousing
tags:
  - Data
---
# Slowly changing dimensions
1. Type 1 overwrites the existing value with new value and does not retain history

    |  Name    |   ID    | Description |
    | -------- | ------- | ----------- |
    | laptop  | 0    |        xxxx     | 
    | phone | 1    |          yyy      |

    <div style="text-align: center; font-size: 24px; font-weight: bold;">⇓</div>
    &nbsp;

    |  Name    |   ID    | Description |
    | -------- | ------- | ----------- |
    | laptop  | 0    |        zzz     | 
    | phone | 1    |          yyy      |

2. Type 2 add a new row for the new value and maintains the existing row for historical and reporting purposes

    |  Name    |   ID    | Description |
    | -------- | ------- | ----------- |
    | laptop  | 0    |        xxxx     | 
    | phone | 1    |          yyy      |

    <div style="text-align: center; font-size: 24px; font-weight: bold;">⇓</div>
    &nbsp;

    |  Name    |   ID    | Description |
    | -------- | ------- | ----------- |
    | laptop  | 0    |        xxx     | 
    | phone | 1    |          yyy      |
    | laptop  | 0    |        zzz     | 

3. Type 3 allows storage of both current and previous value of an attribute in the same row, also it has the limited historical tracking. Last description go to prev Description column, updated description goes to current description column, change date reflects the modify time.

    |  Name    |   ID    | Prev Description | Current Description | Change date |
    | -------- | ------- | ---------------- | ------------------- | ----------- |
    | laptop  | 0    |        xxxx     |        zzz               |  2024-04-29 |
    | phone | 1    |          yyy      |        mmm               |  2024-05-01 |


# Term
1. Partition table
   - Queries scan fewer rows due to partition pruning
   - You can drop or archive old partitions without touching newer data.
   - PostgreSQL can process multiple partitions in parallel.
    ```
    CREATE TABLE sales (
    id SERIAL,
    region TEXT,
    sale_date DATE,
    amount NUMERIC
    ) PARTITION BY RANGE (sale_date);

    CREATE TABLE sales_2025_01 PARTITION OF sales
    FOR VALUES FROM ('2025-01-01') TO ('2025-02-01');

    CREATE TABLE sales_2025_02 PARTITION OF sales
    FOR VALUES FROM ('2025-02-01') TO ('2025-03-01');
    ```
2. Local/Global Index
   - local index：each partition has its own separate index and are physically independent per partition
   - one single index that covers all partitions together. Not supported in PostgreSQL (as of 2025)

 # Miscellaneous
 the copy query in postgresql will not trigger default value of a column, will only show errors. Only insert statement can trigger.