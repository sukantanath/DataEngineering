### DATA WAREHOUSE

Strcutured Data -> ETL -> DWH -> BI
- SQL only interface
- no / less support for ML use case
- streaming / realtime not supported
- **GREAT** for BI

### DATA LAKE

Structured / Semi-Structured / Un-structured data -> Data Lake -> ETL -> ML / Analytics
- **Supports** ML / Analytics
- **Open to any data formats**
- Data quality issue
- Poor Support for BI 
  

**Modern Data Driven Enterprises have 4 data verticals to be taken care of - 
1. Data Warehouse (amazon redshift, azure Synapse, Snowflake, SAP, Teradata, Google Big Query, IBM DB2, Oracle DWH ...)
2. Data Engineering (Hadoop, Amazon EMR, AirFlow,Google Data Proc, Spark, Cloudera ...)
3. Data Streaming (Kafka, Flink, Spark,Amazon Kinesis,Google Dataflow, Azure Stream Analytics, Confluent, Tibco Spotfire ...)
4. Data Science (ML) (Jupyter, Azure ML Studio, Domino Data Labs, Tensorflow, Amazon Sagemaker,Matlab, SAS, PyTorch ....)

Interaction between all these 4 pillars are multi-fold and complex.
Working with diverese tech-stack all together for a business problem is complex . 
Hence development becomes complex and slow as different team owns different pillars and communication between them and moving data between them is a challenge.
Also multiple copies of data resides in system leading to multiple source of truth.

-------------------------------------

**DeltaLake comes in picutre.**

It's the best of DWH & Data Lake.

Structured / Semi-structured / Unstrcutured data -> Data Lake -> Metadata, caching, indeix layer -> BI & ML.

Challenge in data lake -
1. Appends are modificatiosn to existing data are hard
2. Jobs failing midway
3. Mixing streaming & batch leads to inconsidtency.
4. Costly to keep historical data.
5. Hard to manage metadata
6. Too many files problem.
7. Performance
8. Security
9. Data Quality

Delta Lake has - 
1. Curated data lake approach (bronze , silver, gold)
2. Every operation is ACID transaction. Delta lake keeps log for every transaction.
3. Uses Spark under the hood.
4. Indexing , partitoning, data skipping, bloom filter, Z-ordering, auto-optimize.
5. Table ACL
6. Schema Validation & evolution.


### Delta Design Pattern

Ingest (Kafka,spark,Files) => curated data lake (multi hub data i.e. bronze, silver, gold) => AI & Reporting + Streaming 
Silver layer acts as source truth for company.

**The Transactional Layer** is what transforms data lake to delta lake. It index, tracks & manages quality for data.
Databrciks Lakehouse Platform provides all these features. Benefits - 
1. Separation of compute & storage.
2. Infinite Storage.
3. Leverage best practice for data warehouse.
4. No limit on data strcuture.
5. Mix of batch & streaming workloads.
6. High data throughput.


### Core Components of Delta lake
**1. Delta Table**
    collection of data in delta files.
    uses parquert format to store transactional log.
    tables registerd with metastore.
    delta tables supports sql (spark-sql).
    Each atomic commit in transaction log is stored as json transaction in separate file. Timetravel is possible to go through each commit.
    Delta lake saves checkpoint in parquet format for every 10 commits.
**2. Commit Service** 
    Commit service controls how transaction log is run. Changes  / commits contains schema & metadata also.


    Tranasctions are serialized, i.e the order of changes are to be honored.

    Delta lake uses optimistic concurrency (Assume transactions by diff user on same table will be non-conflicting) over Pessimistic Concurrency ( put Lock).
    It uses mutual exclusion to isolate conflicting commits and process them serially in order.