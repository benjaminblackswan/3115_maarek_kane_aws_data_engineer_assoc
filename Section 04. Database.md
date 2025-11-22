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
* automatically scaling and provisioning of Redshift workload
* optimize costs and performance































































































































































































































































