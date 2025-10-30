# 24. Amazon S3
* S3 is one of the main building blocks of AWS
* infinitely scaling

## purpose
* backup and storage
* DR
* archive
* storage
* datalake
* Static website

## Buckets
* S3 is object store
* objects are stored in **Buckets**
* must be globally unique name, ie across all regions
* buckets are defined at region level
* names must be 3-63 characters long

## Objects
* objects files have a key
* key is composed of prefix + object name
* no directories within buckets
* max object size is 5TB
* objects bigger than 5GB must use multi-part upload
* up to 10 tags
* version control

## Hands on

### Creating a new budget
Search for S3 > **Create Bucket**

Note bucket type option is only available in some region, Sydney is not one of them.
<img width="1364" height="135" alt="image" src="https://github.com/user-attachments/assets/45f81c4c-67cc-4d27-ba71-8e4a6762e6aa" />

Bucket name must be unique across all regions

<img width="912" height="67" alt="image" src="https://github.com/user-attachments/assets/9f2da73f-8d68-43ac-8d16-5bf596a093f1" />

Object Ownership: **ACLs** disabled

public access:  Block all public access

Bucket Versioning: Disabled, but will open later.

No tags

Default encryption: Server side with SSE-S3

Bucket Key: enabled

**Create Bucket**

You can see there are two tabs, one for **General purpose** buckets and one for **Directory** buckets.

<img width="948" height="281" alt="image" src="https://github.com/user-attachments/assets/efe79576-c5d7-4d0e-baa8-9b0a21399f68" />


### uploading a file to the bucket

upload the coffee.jpg

### Public vs. Private URL





<img width="1155" height="607" alt="image" src="https://github.com/user-attachments/assets/08c265d5-d16a-4103-947c-dc588a4d7e61" />
























