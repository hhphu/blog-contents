---
title: "AWS - Design for Availability, Reliability and Resiliency"
description: "Desgin AWS Solutions for Availability, Reliability and Resiliency."
date: "2024-11-27"
headerImage: "https://github.com/user-attachments/assets/4258e84d-8e8a-485b-a13d-d857dc430af2"
tags: ["aws", "cloud"]
---

## Create VPCs
Create two VPCs, `primary-vpc` (us-east-1) and `secondary-vpc` (us-west-2):
1. Services -> CloudFormation
2. Create stack "With new resources (standard)"

   [![2024-02-02-11_02_13-stacks-_-cloudformation-_-us-east-1](https://github.com/user-attachments/assets/2ad56ba7-8e12-47ee-b08d-8b90997f62da)](https://github.com/user-attachments/assets/2ad56ba7-8e12-47ee-b08d-8b90997f62da)

3. Upload a template file
4. Click "Choose file" butotn
5. Select the `vpc.yml` file
6. Next
7. Fill in the Stack name, `design-for-arr-primary` & `design-for-arr-secondary`
8. Name the VPCs: `primary-vpc` and `secondary-vpc`
9. Update the CIDR blocks to match the architecture diagram
10. Leave the next options as default and click Submit
11. Wait until the stacks are created

Confirm the stacks are created:

- **design-for-arr-primary**

[![image](https://github.com/user-attachments/assets/8cf43fcf-5f0b-475e-853f-91eb98645d82)](https://github.com/user-attachments/assets/8cf43fcf-5f0b-475e-853f-91eb98645d82)

- **design-for-arr-secondary**

[![image](https://github.com/user-attachments/assets/438e8723-1da2-428e-80b5-8b65ed3d0beb)](https://github.com/user-attachments/assets/438e8723-1da2-428e-80b5-8b65ed3d0beb)

- **primary-vpc**

[![primary_vpc](https://github.com/user-attachments/assets/2f19f795-cf43-4487-9682-adc7c378a3f1)](https://github.com/user-attachments/assets/2f19f795-cf43-4487-9682-adc7c378a3f1)

- **Primary VPC subnets**

[![image](https://github.com/user-attachments/assets/b7f9a750-62e6-4bfc-b103-0ea6571730d5)](https://github.com/user-attachments/assets/b7f9a750-62e6-4bfc-b103-0ea6571730d5)

- **secondary-vpc**

[![image](https://github.com/user-attachments/assets/d6ae0fe0-a748-4ac1-8517-8838bff7938c)](https://github.com/user-attachments/assets/d6ae0fe0-a748-4ac1-8517-8838bff7938c)

- **Secondary VPC subnets**

[![image](https://github.com/user-attachments/assets/542e95df-a4b5-4bbc-9775-9f351481811f)](https://github.com/user-attachments/assets/542e95df-a4b5-4bbc-9775-9f351481811f)

## Data Durability And Recovery
### Create subnet group for DB
We want to place our DB in private subnets. Hence we need to create private subnet groups and attach it to the DB in the process of creating it.
1. RDS > Subnet groups > Create DB subnet group

#### Subnet group details
2. **Name**: `Subnet for primary DB`
3. **VPC**: `primary-vpc`

#### Add subnets
4. **Availability Zones**: `us-east-1a`, `us-east-1b`
5. **Subnets**: Select the two private subnets `primary-vpc Private Subnet (us-east-1a)` and `primary-vpc Private Subnet (us-east-1b)`

[![image](https://github.com/user-attachments/assets/16640088-8a09-4181-b7ff-a305f27416c2)](https://github.com/user-attachments/assets/16640088-8a09-4181-b7ff-a305f27416c2)

### Highly durable RDS Database
In the primary region (us-east-1), create a new RDS Subnet group using the private subnets:
1. Create a new MySQL, multi-AZ database:

[   ![image](https://github.com/user-attachments/assets/4bdb63d3-29e4-43e2-b183-fe7806f201ba)](https://github.com/user-attachments/assets/4bdb63d3-29e4-43e2-b183-fe7806f201ba)

  -  **Settings**
      - **DB instance identifier**: `arr-database`
      - **Master username**: `admin`
      - **Credentials Management**: `Self managed`
      - Enter the **Master password** and **Confirm master password**

 [ ![image](https://github.com/user-attachments/assets/69df495d-8a86-4a7c-8e96-b09b5a59a506)](https://github.com/user-attachments/assets/69df495d-8a86-4a7c-8e96-b09b5a59a506)

  - **Instance configuration**
      - **Burstable classes (includes t classes)** > **db.t3.micro**

[  ![image](https://github.com/user-attachments/assets/fdedf33f-c446-4c3d-9e2b-9baa1ff77bca)](https://github.com/user-attachments/assets/fdedf33f-c446-4c3d-9e2b-9baa1ff77bca)

  - **Connectivity**
    - **Virtual private cloud**: `primary-vpc`
    - **DB subnet group**: `private subnets for db`
    - **VPC security group**: `Choose existing`
    - **Exiting VPC security groups**: `UDARR-Database`

[ ![image](https://github.com/user-attachments/assets/44015f7e-a3ec-4b5c-bde5-9358b3346738)](https://github.com/user-attachments/assets/44015f7e-a3ec-4b5c-bde5-9358b3346738)

  - Expand the **Additional Configuration** section
    - **Initial database name**: `udacity`
    - **Enable automated backups**: `check`
    - **Log exports**: `Audit Log`, `Error Log`, `General Log`, `Slow query log`

[  ![image](https://github.com/user-attachments/assets/2ddc7fd8-a5ee-487c-bb5f-bac583506653)](https://github.com/user-attachments/assets/2ddc7fd8-a5ee-487c-bb5f-bac583506653)

Once the Database is created. Confirm some of its configurations:
- The Primary is only in the private subets:

[![primaryDB_subnetgroup](https://github.com/user-attachments/assets/625b2ea0-9724-4057-aea6-825a6d1c4dc9)](https://github.com/user-attachments/assets/625b2ea0-9724-4057-aea6-825a6d1c4dc9)

- The Primary data has automatic backup enabled:

[![primaryDB_automaticbackup](https://github.com/user-attachments/assets/f6e7dafd-bbc8-4277-85a3-9b8219ef73e6)](https://github.com/user-attachments/assets/f6e7dafd-bbc8-4277-85a3-9b8219ef73e6)

2. Create a read replica database in the standby region with the same requirements
- Select the primary database > Actions > Create read replica

[  ![image](https://github.com/user-attachments/assets/9f0a7ed3-3b60-4099-bb05-d7afb1ce381e)](https://github.com/user-attachments/assets/9f0a7ed3-3b60-4099-bb05-d7afb1ce381e)

- **Settings**
     - **DB instance identifier**: `arr-database-replica`
- **Instance configuration**
     - **Burstable classes (includes t classes)** > **db.t3.micro**
- **AWS Region**
      - **Destination Region**: `US West Oregon`

[![image](https://github.com/user-attachments/assets/34763dd4-50c0-46ad-9398-17cf0bcbfee4)](https://github.com/user-attachments/assets/34763dd4-50c0-46ad-9398-17cf0bcbfee4)

- **Connectivity**
     - **DB subnet group**: `private subnets for db` (make sure to create this subnet group first in the secondary region)
     - **Exiting VPC security groups**: `UDARR-Database`

  [![image](https://github.com/user-attachments/assets/53cf7c70-db7d-46b2-8746-b2dbc0a8886a)](https://github.com/user-attachments/assets/53cf7c70-db7d-46b2-8746-b2dbc0a8886a)

     - **Log exports**: `Audit Log`, `Error Log`, `General Log`, `Slow query log`

  Confirm that the replica databas is also in the private subnet

  [![image](https://github.com/user-attachments/assets/9a7ad303-493e-4908-a569-12fa5acf8bfc)](https://github.com/user-attachments/assets/9a7ad303-493e-4908-a569-12fa5acf8bfc)

### Availability Estimate
- Single AZ outage: In an event of a single AZ outage, we can start a replica in a different zone (within the same region). This will takes about 2 minutes so the RTO is about 2 minutes.
Because the replica is already available and ready to be used, the RPO is about 1 or 2 minutes or, in some cases, can be considered 0 minute.
- Single region outage: In an event of a single region outage, we can refer to the secondary replica from different region. Time to switch to the secondary resources is approximately 5 to 10 minutes. Hence, the RTO is about 5 - 10 minutes.
- Similar to the above scenario, the RPO in this case will be about 5-10 minutes as it's the length for the secondary resources to be ready for usage.

## Create EC2 Instance to access DB
In the primary region `us-east-1`, create an EC2 instance:
1. EC2 > Instances > Launch an instance
2. Select Ubuntu as the OS images option
3. Create a key pair: `ec2-arr.pem`
4. **Network settings**:
  - **VPC**: `primary-vpc`
  - **Subnet**: use one of the VPC public subnets
  - **Security Group**: `Select exiting security group` > `UDARR-Application`
5. Launch Instance
Once the EC2 instance is created, sign in and try to connect to the DB.

```bash
ssh -i ec2-arr.pem ubuntu@$IP
```
## Accesss the primary Database
Access the DB: 

```bash
mysql -u admin -p -h $DB_ENDPOINT
```
[![image](https://github.com/user-attachments/assets/0380120b-09de-49da-bac2-03f998a59da6)](https://github.com/user-attachments/assets/0380120b-09de-49da-bac2-03f998a59da6)

Create a table `users` in the `udacity` database

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT, -- Auto-incrementing ID for unique identification
    name VARCHAR(50) NOT NULL,         -- User's name
    email VARCHAR(100) UNIQUE NOT NULL, -- User's email
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- Timestamp of creation
);
```

Insert data into the table:

```sql
INSERT INTO users (name, email)
VALUES 
    ('Alice', 'alice@example.com'),
    ('Bob', 'bob@example.com'),
    ('Charlie', 'charlie@example.com');
```
Verify the data is inserted into the newly created table:

```sql
SELECT * FROM users;
```

[![image](https://github.com/user-attachments/assets/4d3a360a-9453-4afd-8604-61a694633b19)](https://github.com/user-attachments/assets/4d3a360a-9453-4afd-8604-61a694633b19)

### Check for DB Connection activities.
1. Select the primary database `arr-database` > Select **Monitoring** tab and view the **Database Connections** dashboard.
2. We can confirm that all the connections to DB were all logged and monitored

   [![image](https://github.com/user-attachments/assets/f99dda89-a91e-4961-8812-1da98623a508)](https://github.com/user-attachments/assets/f99dda89-a91e-4961-8812-1da98623a508)

This has concluded that we have successfully created a primary DB in the primary region, which we have both read and write permissions.

## Access the secondary Database (read replica)
In the secondary region `us-west-2`, create an EC2 instance:
1. EC2 > Instances > Launch an instance
2. Select Ubuntu as the OS images option
3. Create a key pair: `ec2-arr-2.pem`
4. **Network settings**:
  - **VPC**: `secondary-vpc`
  - **Subnet**: use one of the VPC public subnets
  - **Security Group**: `Select exiting security group` > `UDARR-Application`
5. Launch Instance
Once the EC2 instance is created, sign in and try to connect to the DB.

Access the read replica DB:

```bash
mysql -u admin -p -h arr-db-read-replica.cnz6yyv6jplm.us-west-2.rds.amazonaws.com
```
[![image](https://github.com/user-attachments/assets/e7a18cf4-9c25-4ea4-93c8-86c7c08865fa)](https://github.com/user-attachments/assets/e7a18cf4-9c25-4ea4-93c8-86c7c08865fa)

Verify we have read permission on the `arr-db-read-replica`

```sql
use udacity;
select * from users;
```

[![image](https://github.com/user-attachments/assets/811f71a6-ceb8-4ca5-9c89-2e3f32ba0559)](https://github.com/user-attachments/assets/811f71a6-ceb8-4ca5-9c89-2e3f32ba0559)

Try inserting new data to the table

```sql
INSERT INTO users (name, email)
VALUES 
    ('Alan', 'alan@example.com');
```

As expected, we get the error stating the Database only has read permission. We can't write to the database.

[![image](https://github.com/user-attachments/assets/bcbefad6-1cfe-46c9-af91-5a094ee1e7e4)](https://github.com/user-attachments/assets/bcbefad6-1cfe-46c9-af91-5a094ee1e7e4)

To grant write permission to the `arr-db-read-replica`, we promote the database.
- Select the `arr-db-read-replica` database > Actions > Promote

 [ ![image](https://github.com/user-attachments/assets/d74da545-01aa-4a05-b39a-4e68e6c9eb52)](https://github.com/user-attachments/assets/d74da545-01aa-4a05-b39a-4e68e6c9eb52)

Once promoted, the database will grant users write permission. Try inserting data again:

```sql
INSERT INTO users (name, email)
VALUES 
    ('Alan', 'alan@example.com');
```

[![image](https://github.com/user-attachments/assets/27fc517b-5866-4a4f-8b43-b2aff256a331)](https://github.com/user-attachments/assets/27fc517b-5866-4a4f-8b43-b2aff256a331)

We now can write to the replica Database. We can also create another table within the database

```sql

CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT, -- Unique identifier for each order
    user_id INT NOT NULL,                    -- References the id from the users table
    product_name VARCHAR(100) NOT NULL,      -- Name of the product
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Date and time of the order
    FOREIGN KEY (user_id) REFERENCES users(id) -- Ensures user_id exists in the users table
);

INSERT INTO orders (user_id, product_name)
VALUES 
    (1, 'Laptop'),
    (2, 'Smartphone'),
    (3, 'Tablet'),
    (1, 'Headphones');
```

Check the newly created table: `select * from orders;`

[![image](https://github.com/user-attachments/assets/1c1b5367-6d68-43e4-aad7-0f2afc388fb6)](https://github.com/user-attachments/assets/1c1b5367-6d68-43e4-aad7-0f2afc388fb6)

Going back to the **Monitoring** tab, we see the connections to `arr-db-read-replica` database is logged in the **Database Connections** dashboard.

[![image](https://github.com/user-attachments/assets/35bada41-dac3-47a1-b1c0-10e2e517d68a)](https://github.com/user-attachments/assets/35bada41-dac3-47a1-b1c0-10e2e517d68a)

We have now successfully created an architecture with high availability and reliability.

