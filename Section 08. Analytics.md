# 129. AWS Glue
* serverless discovery and definition of table definitions and schema
* custom ETL jobs

<img width="332" height="424" alt="image" src="https://github.com/user-attachments/assets/66cfb5a4-3256-468c-8d3d-9691666cc547" />

Glue crawlers scans data in S3, creates schema, then populates Glue Data Catalog (GDC).
once catalogued in GDC, you can treat the unstructured data like it is structured in
* redshift spectrum
* athena
* EMR
* quicksight

## Glue and S3 Partition
think up front about how you will be querying your data lake in s3 as Glue crawler will extract partions based on that.

# 130. Glue, Hive, and ETL
Glue can also serve as a Hive metastore, **Hive** lets you run sql like queries from EMR.

## Glue ETL
* Glue uses graphic interface and creates spark code underneath (scala or pyspark).
* serverless
* Data Encryption
  - server-side for at rest
  - SSL for transit
* can be event-driven
  - use scheduler or
  - triggers
* can provision additional DPU to increase performance.
* errors reported to CloudWatch

## DynamicFrame
it is like Spark DataFrame but with more ETL stuff.

<img width="204" height="385" alt="image" src="https://github.com/user-attachments/assets/7c571bdc-3e0a-4fc3-8c37-1c67b7b1e65b" />

## Glue ETL - transformations
* bundled transformation
  - drop frield
  - filter
  - join
  - map
* ML transformations
* Format conversions

## Glue ETL - ResolveChoice
deals with ambiguities in a DynamicFrame

# 131. Modifying the Glue Data Catalog from ETL Scripts
* ETL scripts can update your schema and partitions if necessary.
* Adding new partitions
* Updating table schema
* Creating new tables
* Restrictions

# 132. Running ETL Jobs with Bookmarks
Ways to run a Glue job
* time-based schedules (cron)
* Job Bookmarks
* CloudWatch Events

# 133. Glue Costs and Anti-Patterns
## Cost
* billed by the second for crawler and ETL jobs.
* Development endpoints for developing ETL code are billed by the minutes.

## Glue anti-patterns
* Multiple ETL engines: not a good idea, use Data Pipeline EMR
* used to be an anti-pattern, but streaming data on Glue is now okay.

# 134. AWS Glue Studio
* visual interface for ETL workflows.
* visual job dashboard
<img width="409" height="390" alt="image" src="https://github.com/user-attachments/assets/a7bf65d9-2102-4849-89d1-223276720ecf" />

# 135. AWS Glue Data Quality
* integrates into Glue jobs to check data quality rules.
* uses DQDL ([data quality definition language](https://docs.aws.amazon.com/glue/latest/dg/dqdl.html))
<img width="264" height="499" alt="image" src="https://github.com/user-attachments/assets/c9112194-a592-46b3-9507-520546057e4a" />


# 136. AWS Glue DataBrew
* a visual data prep tool (like Power Query)
* 250 ready made transformation
* $1 per session or
* $0.48 per node hour for jobs

# 138. Handling PII in DataBrew Transformations
Methods to deal with PII include
* substitution
* shuffling
* Encryption
  - deterministic
  - probabilistic
* delete
* mask out
* hashing

# 139. AWS Glue Workflows
* Design multi-job, multi-crawler ETL processes run together.
* designed to be used within Glue.
* from console or API.
* Can be triggered
  - schedule
  - on demand
  - EventBridge events

# 140. AWS Lake Formation
* built on top of Glue.
* Load data and monitor data flows.
* Anything Glue can do, Lake Formation can do.

<img width="465" height="307" alt="image" src="https://github.com/user-attachments/assets/5f2f15e0-f843-499b-bf75-bde5398b6354" />
<br>
<img width="315" height="349" alt="image" src="https://github.com/user-attachments/assets/dfaf99a6-e3c6-488c-8289-17a799802fbf" />

# 142. Amazon Athena
* interactive query service for S3.
* Built with presto
* serverless

## 143. Amazon Athena and Glue, Costs, and Security

<img width="565" height="210" alt="image" src="https://github.com/user-attachments/assets/f203f94b-1f10-4a62-b698-64b572156a25" />

### Athena Workgroups
* used to organise users/teams/app.
* integrates with IAM, CloudWatch and SNS

### Athena cost model
* pay as you go
  - $5 per TB scanned
  - successful or cancelled queries count, failed queries do not.
  - columnar format can you 30 to 90% of cost and better performance.
* Glue and S3 are separate charge.

### Athena Security
* access control with IAM, ACL and bucket policies
* Encrypt results at rest in S3.
* TLS encrypts in-transit data.

### Athena anti-patterns
* highly formatted reports > use QuickSight
* ETL > use Glue 

## 144. Athena Performance
### Optimisation
* use columnar format such as Parquet or ORC
* Small number of large files is better than large number of small files for Athena.
* Use partition.

## 145. Athena ACID Transactions
* To implement ACID in Athena, change the table_type to **ICEBERG** in your CREATE TABLE command.
* concurrrent users can make row-level changes.
* compatible with EMR, spark and other iceberg formats.
* time travel operations.

# 146. Apache Iceberg and Athena / EMR / Glue Integration
* Created by Netflix, but later opened sourced.
* a table format for data lakes, petabyte scale.
* ACID compliant
* row level updates and delete for GDPR compliance.
* schema evolution
* time travel
* metadata management

<img width="471" height="369" alt="image" src="https://github.com/user-attachments/assets/79d6f279-8ed7-43c0-8c17-9860678c55ab" />

AWS Glue Catalogue replace **Hive**. 

## 147. Athena Fine-Grained Access to AWS Glue Data Catalog
IAM based database and table level security.

## 148. Apache Spark

<img width="262" height="415" alt="image" src="https://github.com/user-attachments/assets/c3e64086-bcd0-44ea-8291-0c329e4d2f35" />

basically MapReduce is shit, we replaced it with Spark.

* Distributed processing framework for big data.
* has in-memory caching
* optimisation engine for queries
* support python, scala, R and Java
* not made for OLTP

### how spark work under the hood

<img width="451" height="529" alt="image" src="https://github.com/user-attachments/assets/a4e88920-9e0b-4865-abd1-afe42545dff3" />

<img width="689" height="511" alt="image" src="https://github.com/user-attachments/assets/a7746212-72c9-4f3d-be5c-9093f4a1c576" />

* Spark SQL gives the sql interface to the EMR cluster.
* GraphX is not popular, superceded by third party graph processing options.

### Spark Structured Streaming

<img width="201" height="254" alt="image" src="https://github.com/user-attachments/assets/955b4348-7023-45aa-bb03-44351b2e3ffd" />






















































































