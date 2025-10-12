# 5. Types of Data (Structured, Unstructured, Semi-Structured)
* Structured data
  - cvs, relational databases
* Unstructured data
  - videos, text etc
* Semi-structured data - some structures eg tags, hierarchies or other patters.
  - eg. json, xml, log files
 
# 6. Properties of Data
Three Vs of data
* Volome - size of data
* Velocity - The speed of new data is generated, collected and processed.
* Variety - type of data see # 5

# 7. Data Warehouses vs. Data Lakes (and Lakehouses)

## Data Warehouse (DWH)
* for structured data, usually cleaned and processed
* designed for queries and analysis
* use SQL
* **Schema-on-write** - ETL
* Cloud data warehouse include AWS Redshift, Azure SQL etc

## Data Lake
* repositiory for raw data, unprocessed
* can be all three types of data as opposed to only structured data in the case of DWH
* **Schema-on-read** - ELT
* Cloud DL include AWS S3 or Azure Data Lake Storage Gen2 or hadoop

## Date lakehouse
combines datalake house and warehouse
* AWS - **S3 with redshift spectrum**
* Microsoft - Azure Synapse analytics
* Databricks - Lakehouse platform built on top of Delta Lake
* Open source - Delta Lake and Iceberg

## 8. What is a "Data Mesh"?
* individual teams own their data products withnin a given domain.
* federated governance with central standards

# 10. Common Data Sources and Data Formats

## API
* JDBC (Java database Connectivity) : is language dependent but platform independent
* ODBC (Open database Connectivity) : is language independent and platform dependent

## Data Formats

* CSV
  - for structured data
  - for samll and medium datasets
  - human readable
  - widely adopted, good for importing data in and out of spreadsheets.

* JSON
  - for structured or semi-structured data
  - based on key-value pairs
  - human readable

* Avro
  - for large dataset
  - binary format, not human readable
  - apache kafka

* Parquet
  - columnnar storage format
  - best for analytics
  - spark or redshift spectrum

## 11. Quick Review of Data Modeling, Data Lineage, and Schema Evolution

* star schema
* ERD
* data lineage - eg Spline agent
* Schema Evolution

## 12. Database Performance Optimization
* create index
* partitioning
  - for parallel progressing for large dataset
  - spark
* compression
  - columnar compression

## 13. Data Sampling Techniques
* Random sampling
* Stratified Sampling
* Systemic Sampling

## 14. Data Skew Mechanisms

unequal distribution or imbalance of data across nodes or partition
* celebrity problems

### how to address data skew
* adaptive partitioning
* salting
* repartitioning
* sampling
* custom partitioning

## 15. Data Validation and Profiling
* completeness
* consistency
* accuracy
* integrity


# Code exercise

## exercise 1

Practicing aggregation queries in SQL

Women and children first!

You probably know the story of the Titanic, an ocean liner that tragically sank in 1912. A famous policy of the time was "women and children first" when loading up the lifeboats in such a situation.
Does the data show this is what actually happened on the Titanic?

We've loaded up a sample of 100 passengers on the Titanic, which includes their age in years, whether they survived, and their self-reported gender, into a table named titanic. Explore this data, and compute:

* A listing of the first ten rows in the titanic table, to help you understand its structure and column names.
* The overall survival rate of the passengers in this data set. This result should be labeled overall_rate.
* The overall survival rate of "women and children," identified by a gender of 'female' or age of 12 or younger. This result should be labeled women_children_rate.
* The overall survival rate of everyone else who does not fit our definition of "women and children." This result should be labeled others_rate.

Compute these as four separate SQL queries; we're not looking to use grouping here yet.

Did women and children have better odds of surviving the Titanic than others did?

```
SELECT * FROM titanic LIMIT 10;
SELECT AVG(Survived) AS overall_rate FROM titanic;
SELECT AVG(Survived) AS women_children_rate FROM titanic WHERE Age <= 12 OR Sex = 'female';
SELECT AVG(Survived) AS others_rate FROM titanic WHERE Sex != 'female' AND Age > 12;
```

We use AVG(Survived) as a quick way to compute survival rates; since a value of 0 means they did not survive and a value of 1 means they did, the math works out there.

It's potentially confusing that "women and children" actually requires an OR condition to include both sets of people, but that is how we include them both. Note that "children" includes 12 year olds, so it's important to use <= there.

Not we chose to define "everyone else" as having a non-female sex and being over the age of 12, rather than just testing for "male". This ensures we capture people who did not report as male or female, or perhaps left that data blank.

You should have found that the survival rate for women and children was 71%, compared to just 13% for others. They really did load up "women and children first" it seems.

## exercise 2
Did class matter? We have the same sample of 100 passengers on the Titanic, but this time we want to explore if the passenger class of their ticket (first, second, or third class) affected their odds of survival.

Your sample dataset is in a table named titanic. This contains a column named Survived, which is 1 if they survived and 0 if not. There is also a Pclass column indicating their passenger class (1, 2, or 3.)

Your task is to use GROUP BY in SQL to produce the survival rate for each passenger class in our dataset. Your output should contain a table with two columns named Pclass and survival_rate.
The results should be sorted in ascending order by passenger class.

```
SELECT Pclass, AVG(Survived) AS survival_rate FROM titanic GROUP BY Pclass ORDER BY Pclass;
```

Although you could compute survival rate with complicated CASE and mathematical functions, a simple AVG will do - the math works out.

We produce survival_rates for each distinct Pclass using GROUP BY Pclass, and enforce the specified sorting of the results using ORDER BY.


## exercise 3

Practicing join queries in SQL

We've loaded up a couple of tables of data from a fictional retailer: Products, containing information about the products sold by the company, and Suppliers, containing information about the companies that provided those products.
These two tables are connected by columns named SupplierID.

Create a report of every ProductName in the Products table, together with the CompanyName associated with each product's supplier.

This query should be written in such a way that every product is listed in your report, even if no match exists in the Suppliers table for its SupplierID.
Your final results should be sorted alphabetically by ProductName.

```
SELECT Products.ProductName, Suppliers.CompanyName
FROM Products
LEFT JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID
ORDER BY Products.ProductName;
```

We are using LEFT JOIN to fulfill the requirement to produce a row for every product, even if there is no matching SupplierID in the Suppliers table.

ORDER BY is used to produce the specified sort order in the results.


# Quiz 1 Data Engineering Fundamentals recap


<img width="816" height="677" alt="image" src="https://github.com/user-attachments/assets/11670e29-e752-45ec-9042-4e88e6e361f0" />

<img width="827" height="561" alt="image" src="https://github.com/user-attachments/assets/3d761557-5369-4fcd-a338-1664718c6c91" />

<img width="847" height="726" alt="image" src="https://github.com/user-attachments/assets/84a18ecc-3a93-4b7d-b337-ba66a1343463" />

<img width="821" height="642" alt="image" src="https://github.com/user-attachments/assets/82118111-a98a-4f5a-8e9c-0d2788433235" />

<img width="826" height="679" alt="image" src="https://github.com/user-attachments/assets/9b37a5bd-9705-42c4-a207-458c8acda402" />

<img width="838" height="819" alt="image" src="https://github.com/user-attachments/assets/885462ef-e215-4642-a994-e5e45c816102" />

<img width="845" height="609" alt="image" src="https://github.com/user-attachments/assets/45355a70-47fc-4aed-a76b-0f1bd0ce8e7c" />

<img width="837" height="639" alt="image" src="https://github.com/user-attachments/assets/e5ad24e3-36b6-48c4-9a20-ad4a5abb9aca" />

<img width="837" height="573" alt="image" src="https://github.com/user-attachments/assets/59a106c1-68e7-4d40-a86d-01723a68b249" />

<img width="837" height="621" alt="image" src="https://github.com/user-attachments/assets/bc2b8939-4a8e-44ce-8769-7c2d522ff442" />
