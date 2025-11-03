# 129. AWS Glue
* serverless discovery and definition of table definitions and schema
* custom ETL jobs

<img width="532" height="424" alt="image" src="https://github.com/user-attachments/assets/66cfb5a4-3256-468c-8d3d-9691666cc547" />

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



































































































































