# DynamoDB

## 58. Amazon DynamoDB

### Traditional RDBMS 

<img width="616" height="246" alt="image" src="https://github.com/user-attachments/assets/e94055d4-20f4-462e-99c8-dc49475900e8" />

* strong requirements about how data should be modelled
* ability to do query joins, aggregations and complex computations
* uses a dialect of SQL
* horizonal scaling is limited

### NoSQL databases

* non relational
* distributed
* does not support query joins (or very limited)
* all data must be present in one row
* does not perform aggregation
* examples include MongoDB and AWS DynamoDB
* ability to scale horizonally

### Amazon DynamoDB

* fully managed
* replication across multiple AZ
* can scale to massive workloads
* distributed database
* low latency on retrieval

#### Basic of DynamoDB
* made of tables
* each table has a primary key
* rows are called **items**
* columns are called **attributes**
* attributes can be nested and added later
* data types supported are
  - Scalar types: string, number, binary, boolean, null
  - Document types : list, map
  - Set types : string set, number set, binary set

#### how to choose primary keys for DynamoDB

* Option 1 : Partition Key (HASH)
  - partition key must be unique for each item (row)
  - partition key must be diverse

    <img width="355" height="308" alt="image" src="https://github.com/user-attachments/assets/b33d156e-22e1-4cf5-8742-1f4f386071bf" />

* Option 2 : Partition key + Sort Key (HASH + RANGE)
  - the combination must be unique for each item
  - data is grouped by partition key

example of a good **Partition Key** is movie ID (as oppose to producer name, actor name, movie language), because movie_ID is unique for each movie, therefore it has the highest cardinality, therefore unique and diverse.

## 59. Amazon DynamoDB - Hands On

Table name: Users
Partition Key: user_id    string
Sort Key: empty

table setting > Customize settings

#### Table class

DynamoDB Standard vs. DynamoDB Standard-IA, Standard-IA is for infrequently accessed data.

#### Read/write capacity settings

Capacity mode: on-demand vs. provisioned, choose provisioned

Read capacity: auto scaling off 2 CU
Write capacity: auto scaling off 2 CU

Warm throughput: keep default values for both read and write operations

#### Encryption at rest

AWS owned key

**click create table**

<img width="762" height="245" alt="image" src="https://github.com/user-attachments/assets/0ceca2a9-6b9b-4bc0-85a5-7e7b2b2589a1" />

<img width="715" height="133" alt="image" src="https://github.com/user-attachments/assets/65c05e19-63b9-444b-b430-8fa458793709" />


<img width="812" height="649" alt="image" src="https://github.com/user-attachments/assets/8524df0b-a535-4ce2-8884-75fb84ffee59" />

Create our first item

<img width="800" height="455" alt="image" src="https://github.com/user-attachments/assets/ad1f5646-16a1-4c04-9e64-9497e7e1050b" />

Create our second item

<img width="846" height="311" alt="image" src="https://github.com/user-attachments/assets/24cdea2e-f1bd-4179-8736-8cc0eae32bdf" />

Now if you have a look at the table, you will see that all three attributes are there in the table, it just show nulls for the items without the attribute.

<img width="1216" height="273" alt="image" src="https://github.com/user-attachments/assets/b5f0d257-4299-4ab6-9ea0-f832a1ac7eb6" />

Lets create a second table called **UserPosts**

partition key: user_id - string
sort key: post_ts  - string

<img width="819" height="251" alt="image" src="https://github.com/user-attachments/assets/28d36ed6-0f76-442e-b0bf-38cdd03799a0" />

## 60. Amazon DynamoDB in Big Data

anti pattern for DynamoDB
* prewritten application tiet to traditional relational db, use RDS
* joins or complex transactions.
* for blob data, store in S3


## 61. Amazon DynamoDB - Throughput (RCU & WCU)

### Provisioned vs. On-demand
* Provisioned mode
  - You need to plan capacity beforehand
  - pay for the provisioned read and write capacity units
  - cheaper
* On-Demand mode
  - automatically scale up or down with your workloads
  - no plannig needed
  - more expensive than provisioned mode

### Provisioned

* tables must have provisioned read and write capacity units (RCU and WRU)
* there is option to set up auto-scaling of throughput
* Burst capacity can exceed throughput temporarily

#### Write Capacity Unit WCU
one WCU represents one write per second for an item up to 1 kb in size.

#### Read Capacity Unit RCU

* strongly consistent read vs Eventually consistent read

One RCU represents one Strongly Consistent Read per second, or two Eventually Consistent Reads per second, up to 4 KB in size


### Partitions internal
* data is stored in partitions
* partition keys go thourgh a hashing algo to know which partion they go to
* WCU and RCU are spread evenly across partitions
* if you exceed provisioned RCUs and WCUs, then you will get _ProvisionedThroughputExceededException_
  - Exponential backoff
  - Distributed partition keys
  - use DynamoDB Accelerator

### On-Demand

* Unlimited WCU and RCU, no throttle and more expensive
* charged in terms of Read Request Units (RRU) and Write Request Units (WRU)
* about 2.5 times more expensive than provisioned capacity

### To edit RCU or WCU, go to 

<img width="1239" height="364" alt="image" src="https://github.com/user-attachments/assets/b01f0c65-904b-4875-8f2a-727b7c0273a0" />

You can even change the capacity mode 

<img width="1792" height="205" alt="image" src="https://github.com/user-attachments/assets/924fc086-dffd-4bff-baf6-7425c1d8defc" />

## 63. Amazon DynamoDB - Basic APIs


### Writing Data

**Putitem** : create new item or fully replace an old item (same as Primary Key), consumes WCUs

**UpdateItem** : Edits an existing item's attributes or adds a new item if it doesn't exist

**Conditional Writes** : 

### Reading Data

**GetItem** : Read based on Primary key

Query
* keyConditionExpression
* FilterExpression

Scan
* scan the entire table and then filter out data

### Deleting Data

**DeleteItem** : delete an individual item
**DeleteTable** : delete the whole table  without scanning

### Batch Operations

#### BatchWriteItem

#### BatchGetItem

#### PartiQL

## 63. Amazon DynamoDB - Basic APIs - Hands-On

<img width="304" height="383" alt="image" src="https://github.com/user-attachments/assets/f77cc7ed-fec4-455c-a9df-bfa16ae8682c" /> <br>

<img width="473" height="268" alt="image" src="https://github.com/user-attachments/assets/9a2a2002-7181-4b68-bc67-73802b80ed3d" /> <br>

<img width="534" height="168" alt="image" src="https://github.com/user-attachments/assets/723fd0c2-4d5e-40c1-959a-ca5ebdfaa3df" /> <br>

<img width="1568" height="433" alt="image" src="https://github.com/user-attachments/assets/ce497893-c0bf-4213-825e-ba00f24962a7" /> <br>

Click on scan, which will scan the entire table.

Lets create another item

<img width="842" height="435" alt="image" src="https://github.com/user-attachments/assets/b2115400-635f-474c-861a-80e43b0fba16" />

This is called a **PutItem**


## 65. Amazon DynamoDB - Indexes (LSI & GSI)
### Local Secondary Index (LSI)
* Alternative sort key
* up to 5 LSI per table
* must be defined at the table creation


for example, if you want to sort this table by user_id and game_ts, you must create a LSI based on Game_TS.

<img width="718" height="329" alt="image" src="https://github.com/user-attachments/assets/eae56bd5-fd3c-4652-9f02-4e4a1354de49" />

### Global Secondary Index (GSI)
* alternative primary key, ie HASH + RANGE from the base table.
* must provision RCU and WRU for index
* can be added or modified after table creation.

<img width="789" height="311" alt="image" src="https://github.com/user-attachments/assets/42f7f530-5ce6-417c-acd6-194ad46fe29b" />


### GSI vs. LSI throttling
#### GSI
* if the writes are throttled on the GSI, then the main table will be throttled.
* even if the WCU on the main tables are fine

#### LSI
* uses the WCUs and RCUs of the main table
* no special throttling considerations


### Indexes (LSI & GSI) hands-on

Create a new table, there will be an option to create local index or global index

<img width="821" height="330" alt="image" src="https://github.com/user-attachments/assets/0bd2f130-c201-48a4-adaa-9c91e04e53f2" />

## 67. Amazon DynamoDB - PartiQL

use a sql like syntax to manipulate DynamoDB table

<img width="227" height="350" alt="image" src="https://github.com/user-attachments/assets/99e27d00-c107-4c71-83ca-bc012f32f254" />

































































































# Redshift

## 84. Amazon Redshift Intro & Architecture

* fully-managed, petabye scale data warehouse
* designed for OLAP
* MPP

### Redshift Architecture
<img width="478" height="444" alt="image" src="https://github.com/user-attachments/assets/5f6d794e-a43e-4093-9da4-a340289053cd" />


* A cluster of composed of a **Leader node** and one or more (between 1 to 128) compute nodes.
* Each cluster can hold one or more databases.
* user data is stored on compute nodes
* Leader node manages communication between client and compute nodes, leader note sends execution plans to compute nodes and aggregates intermediatary results from compute nodes.
* Each compute nodes are divided into slices.
* Each compute node have its own cpu, memory and storage.

## 85. Redshift Spectrum and Performance Tuning

### Spectrum
* Redshift Spectrum (RS) query exabytes of unstructured data in S3 without loading into a Redshift cluster.
* Separation of storage (S3) and compute (Redshift cluster)

### Performance Tuning
why is Redshift so fast
* Massively Parallel Processing
* Columnar data storage (columnar is best for analytics), therefore Redshift is not suited for OLTP
* Column compression
* Indexes and Materialised Views (MV) are not required for Redshift, therefore Redshift uses less space than traditional row based RDBMS

## 86. Redshift Durability and Scaling

### 86.1 Redshift Durability
* data is automatically replicated within a Redshift cluster.
* data is continuously replicated to S3.
* therefore there are three copies of data, original, cluster copy and S3 copy.
* Single node redshift do not support replication within the cluster, in this case, you must restore from the S3.
* AWS will automatically replace a failed node in a cluster, when it is being replaced, the cluster is unavailable for queries or updates, must recent data is replaced first.
* AWS recommended a minimum of two nodes for production clusters.
* Multi-AZ support is available only for RA3 cluster.

### 86.2 Redshift Scaling
* can do both vertical and horizontal scaling on demand
* during scaling, a new cluster is created, while old one remains available for read, CNAME is flipped to new cluster, data movde in parallel to new compute nodes.

## 87. Redshift Distribution Styles 
there are four distribution styles
* Auto - Redshift figures it out based on size of data.
* Even - Rows distributed across slices.
  
  <img width="245" height="450" alt="image" src="https://github.com/user-attachments/assets/b2a13fc0-719c-4e26-a2aa-5ad1ab8977ae" />

* Key - Rows distributed based on one column.
  
  <img width="239" height="464" alt="image" src="https://github.com/user-attachments/assets/359d32bb-2a23-4b9b-93e5-8301e69eff7d" />

* All - Entire table is copied to every node, the size of the data is multiplied by the number of nodes.

  <img width="266" height="454" alt="image" src="https://github.com/user-attachments/assets/6597b125-88df-4cd5-bd68-5e34293de83b" />

To find out distribution style, query the reflective style column in PG class info.

## 88. Redshift Data Flows and the COPY command

### COPY command
* To get data into Redshift use the COPY command
  - parallelised, efficient
  - from S3, EMR, DynamoDB
  - S3 requires a **manifest file** and IAM role.
* To get data out of Redshift, use the UNLOAD command, from a redshift table into files in S3
* Auto-copy : automatically copies data from new data from S3 into Redshift.
* Amazon Aurora zero-ETL integration : similar to auto-copy but for Aurora
* Redshift Streaming Ingestion : similar to auto-copy but for Kinesis or MSK.

Remember COPY is for external data outside Redshift, for internal Redshift data, use
* INSERT INTO ...SELECT or (insert data into existing table)
* CREATE TABLE AS (creating a new table)

* COPY can decrypt data from S3
* Compression supported

if you have a **narrow** table, load with a single COPY transaction.

### cross-region snapshot copies
you want to copy a snapshot to another region for backup
* in the destination AWS region
  - create a kms key
  - specify a unique name for your sanpshot copy grant
  - specify the KMS key ID for which you're creaoting the copy grant
* in the source region
  - enable copying of snapshots to the copy grant you just created.

### DBLINK
connect Redshift to PostgreSQL







## 89. Redshift Integration / WLM / Vacuum
* S3
  - export parallelly to multiple S3 files, import from S3 or even sit on top of S3 using Redshift.
* DynamoDB
  - using COPY command to load data from DynamoDB into Redshift
* EMR / EC2
  - using AWS data pipeline and DMS to move into Redshift

### Redshift Workload Management (WLM)
* create different WLM queues for short and long running queries.
* set up using CLI, console or API

### Concurrency Scaling
* Automatically adds cluster capacity to handle increase in concurrent read queries.
* support virtually unlimited concurrent users and queries.
* use WLM queues to manage which queries are sent to the concurrency scaling cluster


### Automatic Workload Management
* create up to 8 queues
* default 5 queues with even memory allocation


### Manual Workload Management
* 1 default queue with concurrency level of 5
* one superuser queue with concurrency level of 1
* define up to 8 manual queues, concurrency level of 50


### Short Query Acceleration (SQA)
* prioritize short-running queries over longer-running ones.
* can be used to replace Workload Management


### Vaccum command
* recovers spaces from deleted rows
* four types of VACUUM command
  - VACUUM FULL
  - VACUUM DELETE ONLY
  - VACUUM SORT ONLY
  - VACUUM REINDEX
  

### Redshift anti-patterns

you should **NOT** use Redshift for the following situations
* Small datasets > use RDS
* OLTP > use RDS and DynanoDB
* Unstructured data > use EMR
* Blob data (object storage) > use S3


## 90. Redshift Resizing

* Elastic resize
  - quickly add or remove nodes of the same type
  - designed to minimise downtime, few minutes
* Classic resize
  - allows you to change node type and number of nodes
  - downtime can take hours or even days
  - to reduce downtime in classic resize operation, we can use **Snapshot, restore and resize** techniques

## 91. RA3 Nodes, Cross-Region Data Sharing, Redshift ML

* RA3 nodes
  - this node is designed specifically for Redshift
  - compute and storage have been decoupled, so that compute and storage can scale independently in RA3 node type
  - Storage is managed, and is SSD based
  - Allows **Cross-region data sharing**, share live data across regions
* Redshift data lake export
  - unload Redshift query to S3 in **Parquet** format
  - Parquet is 2x faster to unload and consumes up to 6x less storage
  - compatible with Redshift Spectrum, Athena, EMR and SageMaker
* Spatial data types: geometry and geography
* Redshift ML

  - <img width="626" height="462" alt="image" src="https://github.com/user-attachments/assets/6c5aa145-ed9e-47c3-8899-966a29bc3f66" />

## 92. Redshift Security

* Using Hardware Security Module (HSM)
  - use a client and server certificate to establish connection between Redshift and HSM
  - must create encrypted cluster first before moving in data
* Defining access privilegs for user or group
  - use GRANT or REVOKE command in SQL

## 93. Redshift Serverless
* Automatically scaling and provisioning of Redshift workload
* Optimize costs and performance using ML
* Easy spin up of environments and ad-hoc business analysis
* serverless endpoint or query via consoles's query editor.

### Getting started with Redshift Serverless
* Must create IAM role manually with allow on action **redshift-serverless**
* must be fine the following
  - database name
  - admin user credentials
  - VPC
  - encryption settings
  - audit logging
* Can manage snapshots and recovery points


### Resource Scaling in Serverless
* Billed in terms of **Redshift Processing Units** (RPU), as oppose to servers
* RPU hours plus storage
* you can set base RPU and max RPU

### Redshift serverless can **Not** do the following
* parameter groups
* workload managemnet
* partner integretaion
* maintenance windows
* version tracks
* no public endpoint

### Redshift Serverless monitoring
* Monitoring views
* CloudWatch logs
* CloudWatch metrics

## 94. Redshift Materialized Views

* It is precomputed results based on sql queries over one or more base tables, the result of the query is stored on the database
* This can speed up the query result as the data does not need to computed during run time.
* Data in materialized view must be explicitly refreshed if the underlying tables' data change.

## 95. Redshift Data Sharing / Data Shares
* Securely share live data across Redshift clusters for read purposes
* good for workload isolation, cross group collaboration, sharing data between environments
* Clusters must be encrypted and on RA3

## 96. Redshift Lambda UDF

* Use custom functions in AWS Lambda inside SQL queries
* tying up Lambda with Redshift using UDF
* using AWSLambdaRole IAM policy to grant permissions to Lambda on your cluster's IAM role

## 97. Redshift Federated Queries

* Query and analyse across databases, warehouses and data lakes
* Ties Redshift to RDS or Aurora
* avoids the need for ETL
* Must be in the same VPC or using VPC peering
* Credentials must be in AWS secrets manager
* include secrets in IAM roles for your Redshift cluster
* Read only access

<img width="319" height="462" alt="image" src="https://github.com/user-attachments/assets/999bd717-0a14-48c7-8f19-6332682810f0" />

## 98. Redshift System Tables and System Views
* SYS views
* STV tables
* SVV views
* STL views
* SVCS views
* SVL views

## 99. Redshift Data API

Secure HTTP endpoint for SQL statements for redshift clusters

<img width="526" height="137" alt="image" src="https://github.com/user-attachments/assets/b09ceede-01e1-4176-ba82-69d82ccf30c4" />










































































































































































































































