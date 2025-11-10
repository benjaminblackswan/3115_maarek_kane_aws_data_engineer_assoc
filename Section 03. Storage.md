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

## 25. S3 Hands-on

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

Click on **cofee.jpg** to see Object Overview

### Public vs. Pre-Signed URL

The **Object URL** you see in Object Overview is the public URL, if you click on the copy icon, and paste it in the new tab, you will get an error message.

<img width="870" height="376" alt="image" src="https://github.com/user-attachments/assets/4ad584cd-184f-45d0-adf7-b65694a5c0e7" />

<img width="954" height="193" alt="image" src="https://github.com/user-attachments/assets/cd8037fa-8f03-4008-ac63-91d8bc11ae9e" />

However if you click on **Open** icon, you can see the picture in a new tab, this is because AWS creates a private URL and opens it in a new tab.

<img width="1436" height="246" alt="image" src="https://github.com/user-attachments/assets/5ea3b065-ebfc-4d13-b2aa-280ac11337ec" />

<img width="1260" height="836" alt="image" src="https://github.com/user-attachments/assets/6808cd8e-eb98-44c7-a13e-e888b02e7666" />

The private URL might look the same, but if you click on the address bar, it will display the private parts after the public url. 

<img width="868" height="335" alt="image" src="https://github.com/user-attachments/assets/7917224d-689c-42a2-88d4-f25c58996f1a" />

<img width="1264" height="116" alt="image" src="https://github.com/user-attachments/assets/170b7d11-4873-4e79-9096-5c8e9d9f5980" />

This private part is called **S3 pre-signed URL**.

### Create Folder

At the Bucket level, click **Create Folder**

<img width="383" height="98" alt="image" src="https://github.com/user-attachments/assets/eb03d6da-44d8-46b9-b14f-ff0c3c718f34" />

# 26. Amazon S3 Security - Bucket Policy

There are two types of Security in S3, User-based and Resource-based.

* User-based
  - IAM policies

* Resource-based
  - Bucket policies - buket wide rules from s3 console
  - Object Access Control List
  - Bucket Access Control List

an IAM pricipal can access an S3 object if
* the user IAM permissions allows it or resource policy allows it
* **AND** there is no explicity DENY

## S3 Bucket policies
* JSON based

<img width="498" height="410" alt="image" src="https://github.com/user-attachments/assets/7731d751-850e-44cc-a983-3cb3048ac5e4" />

































# 52. Amazon EFS (Network File System)

<img width="39" height="168" alt="image" src="https://github.com/user-attachments/assets/06e8746c-f5d6-4527-bd2c-1f7133e8e153" />

* storage that can be mounted to many EC2
* EFS works with EC2 instances in multi-AZ
* very expensive
* Compatible with only Linux based AMI in EC2

<img width="354" height="252" alt="image" src="https://github.com/user-attachments/assets/a1b70027-a473-4ead-8ff1-bcaeb8bd63dd" />


### EFS classes
* Performance mode
  - General purpose (default)
  - Max I/O
* Throughput mode
  - Bursting
  - Provisioned
  - Elastic
* Storage Tiers
  - standard
  - infrequent access
  - archive

<img width="204" height="601" alt="image" src="https://github.com/user-attachments/assets/28042a15-34e8-407f-98c5-e2cf30a1231e" />

* Availabililty and durability
  - Standard
  - One Zone


## 52. EFS Hands-on

### Create file system

#### General 
* Name - leave empty
* VPC - default

Click on **Customize**
* File system type
  - Regional: for multiple Az, good for production environment.
  - One Zone: for one AZ to save money.

**Automatic backups**: enabled.

**Lifecycle Management**: 

<img width="467" height="142" alt="image" src="https://github.com/user-attachments/assets/9da51688-0bfc-487a-94c3-4568215151d3" />

**encryption** : Enable

#### Performance Settings
* Throughput mode: Enhanced - Elastic
* Performance mode: General Purpose (GP)

#### Network acccess

VPC - same as before, choose default

**Mount targets**

The SG we have before does not have the necessary rules that wwe need for our efs, we only have the default SG.

<img width="729" height="534" alt="image" src="https://github.com/user-attachments/assets/4238a01a-4c09-4bb6-8408-673d16360e24" />


Therefore, We need to create a sg first for our efs


<img width="558" height="170" alt="image" src="https://github.com/user-attachments/assets/9cba83ca-9e84-43bb-b9ff-10edf73dbd34" />

SG name: _efs-demo_

click **Create**


Go back to step 2. Network access of EFS

remove all the default SG from all the AZ and choose efs-demo for all three AZ

<img width="743" height="547" alt="image" src="https://github.com/user-attachments/assets/d24a2d8a-6f04-4eea-8042-0f817f20d572" />

#### File System Policy

leave as it is

and click **create**

<img width="772" height="350" alt="image" src="https://github.com/user-attachments/assets/72ba0a47-8470-47c7-b181-215d447d5de7" />

Click on the EFS, and you can see that it is using 6kb

The next step is to mount the EFS we just created to an EC2 instance.

#### Creating an EC2

Name : Instance A

use Amazon Linux

AMI : Amazon Linux 2023

instance type: t3.micro

Allow SSH traffic from **Anywhere 0.0.0.0/0**


<img width="527" height="414" alt="image" src="https://github.com/user-attachments/assets/5516371f-e866-49c4-9c1e-f1e48f93705b" />


#### Configure storage

First choose our subnet in **Network settings**

<img width="417" height="241" alt="image" src="https://github.com/user-attachments/assets/1203cdb8-5776-48f3-805d-f6ed00fa0581" />


previously used EBS, now we can use EFS we just created, click on edit to add **file system**, add EFS file system

<img width="547" height="414" alt="image" src="https://github.com/user-attachments/assets/fed3a2ed-8a17-44db-a591-be5c483c7e28" />

**add shared file system**

choose EFS

<img width="502" height="229" alt="image" src="https://github.com/user-attachments/assets/957445e6-d3f0-46b4-b73a-478e1f771c88" />


### Create a second instance (9:00)

name: instance B 

everything else the same


go to EFS console > Network tab

you will see all the SG for each AZ in the EC2.

<img width="905" height="375" alt="image" src="https://github.com/user-attachments/assets/815c4957-4aa7-4aa6-be57-417ea43fbf25" />

## testing the instances and the NFS

connect to Instance A and do the following. **note: it is fs1 (number 1), not fsl**

<img width="738" height="534" alt="image" src="https://github.com/user-attachments/assets/3eff031c-0895-43ff-bb0c-9b6a04f06ce2" />

we created a text file called _hello.txt_

Lets query that in Instance B

<img width="617" height="324" alt="image" src="https://github.com/user-attachments/assets/3ac6fc04-c819-4c53-a19a-fbb11a645e38" />

as you can see, both instances are sharing the same NFS from two different AZ.































































































