# assessmentRDS
# FOR Multi-regional High-Availability I have approached this Architecture:
 
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/89c66c93-58e1-4823-aab7-594cca23eea6)


# FOR High-Availability (This will make the system resilient)
We use 
# 1.	Multi-AZ DB Instance Deployment
# 2.	Multi-AZ DB Cluster Deployment

# For Disaster Recovery: (Complete Failure in one region doesn’t affect the service to get down), though there might be 1-2 minutes of down-time in the service.
We use 
1.	RDS Backup – Taking the backups of RDS Instance (Select same or different region and the Encryption Method) – restore it from backup.

2.	Read Replicas (It will have the different RDS Endpoint), so manual pointing will be required and we need to promote it to Primary.

This task can be automated by using the Lambda Function, this will automatically change the RDS Endpoint from Mumbai to Singapore in application.


# General Deployment Options:
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/81787a4c-5634-41a9-83ea-ba1e32099c67)


# STEPS for RDS MULTI-AZ DB CLUSTER, WITH READ-REPLICA and BACKUP
1.	Create a VPC with 1 Public Subnet and 2 Private Subnet. (2 private subnets in different AZ’s)
2.	Attach an Internet Gateway IGW to the VPC created.
3.	Create an RDS DB Subnet Group consisting 2 Private Subnet.
4.	Launch EC2 Instance in Public Subnet and connect it over SSH.
5.	Launch MYSQL RDS instance in Multi-AZ Deployment mode.
6.	We install mysql client and connect it to RDS Instance using RDS DNS Endpoint.
7.	Let’s create a table and insert some records.
8.	Lets note down the AZ in which the RDS Master Instance is running. 
9.	Reboot the RDS instance with option “Reboot with Failover”
10.	Wait for few seconds and verify if you can still connect the same RDS DNS Endpoint via mysql client from EC2 instance.
11.	Verify if you can see the same records in the database table.
12.	Wait for few minutes and verify the AZ in which the Primary or Master is running.
(See console, See Events)
13.	Similarly click the DB, and click modify, to create a Read Replica,(We can create this is different region)
14.	In-case to Primary region becomes down, we can Promote the Read Replica. This will help in functioning it as a master(read/write operations)
15.	In RDS Console we go to the Backup section, where we can continuously take the backup of the database instance to s3. 
16.	In-case of failure, we can restore the database for the earliest point of backup taken.

# During AWS RDS Multi-AZ DB Cluster Deployment we have basically few options to be set while configuring it. Like
•	Database Engine type and Engine Version
•	DB Templates Like Prod, Dev/Test and Free-tier
•	Deployment Options, Multi-AZ’s or Standalone-single DB instance
•	DB instance Identifer
•	Credential for Login DB instance
•	DB Instance Class (like Standard Class, Memory optimized or Burstable class)
•	Storage Type – General (min 20GB) or Provisioned IOPS (min 100GB) to 65TB.
•	Storage Autoscaling 
•	Connectivity and Public Access Option
•	DB Subnet Group
•	Monitoring option
•	Backup Plan
•	Encryption
•	Maintenance Mode
•	Log Management and many more

# PROCESS EXPLAINED:
# Step 1: From AWS Console navigate to RDS Dashboard. 
 
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/680a4479-cfd0-4b6a-b324-4cd2198f7bc0)


# Step 2: Mark to Standard Mode/MySQL
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/44654ec3-84cb-4191-8744-10c7d9fff22f)


# Step3: Choose Template Mode and Deployment Option as Multi-AZ DB Cluster or Instance, as your need.
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/e86f2edb-45ae-4894-b9a0-a218b1eefce4)

 
# STEP4: Select DB Instance Configuration and Storage type(General / Provisioned IOPS) as your need.
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/ec8d5822-63aa-4f7b-b544-85cb03c3b1ed)


# STEP 5: CREATE DATABASE:

 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/085388c4-c2d9-4ae1-915c-2fcede402863)
      
Your Dashboard will indicate that your DB instance is in Creating State.
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/78f55018-04d4-47a7-ba20-f0078b086b37)


# Step 6: Install MYSQL WorkBench in your PC, or mysql-client in your linux-box.

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/094e755f-19bb-43a8-b18e-8ae705e40a29)
 
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/2544a488-a34d-4684-b6d8-a7640f88ba24)

# STEP 7: Use the Database Endpoint to login, using the credentials used while setting up the database instance. We can make multi queries to the Database as figure below.
 
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/fb67e04e-0007-43b8-bb1e-5bcd6f04cd34)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/325a642a-8036-445c-bec7-1f811e2381b2)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/e7b23fed-08f6-4109-9998-55b6484a155d)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/d45e58ad-ddc2-419b-b0c3-a7e99d0237a4)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/2625f66f-b7d6-4675-95bd-d978e0123e22)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/f59af96b-7d68-483a-900d-3e7c21cb7a35)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/f039bff7-cd77-48d0-bb93-3ee85e384a36)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/8f0cf33c-0160-4dfd-8349-cd237fe1aa4e)


# STEP 8: We can see this database is hosted in US-EAST region on 1d Az.
 
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/3e4c698a-691f-43bb-8556-40d44f52b9f5)


# Step 9: Now we reboot the instance with the option: Reboot with failover.
   
![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/a5b6ad65-0937-483f-ba33-3dc3203185cf)
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/9a152698-ca90-449a-a36e-0d06ee1c96c9)

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/ecdd4282-d73b-4285-b96c-76427ee8657c)
Initially we are listening from the instance with IP: 34.201.254.5

# STEP 10: After 1-2 minutes, we can see different IP: 44.215.228.30 is responding when we are pinging the Public RDS Endpoint.

 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/c8137041-db81-4b2c-86c9-243b371f55b9)

This is because the Standy-by database instance has taken the role of Master, which is on the different AZ’s i.e us-east-1f.

![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/ba4a6518-8248-4146-8bc6-3d37e5e34d9f)
 
After the reboot is completed, we can again see the Region and AZ as the previous one.
 
 ![image](https://github.com/gunjankhanal/assessmentRDS/assets/20742236/13614bf7-510b-4623-a8b0-91253d0e2f41)

# STEP 11: On Clicking the ACTIONS----- CREATE READ REPLICA. (Select the different Region and way of Encryption )
We can create the read replica which can be helpful in handling the read-only queries to the database. It will have a different RDS Endpoint. In-case of Region Failure, we need to change in the DNS Lookup maintaining the CName. Also we should to Promote this Read-Replica, this will help in making Primary Database. All read/write operations can be handled after doing this tasks defined. This is the Manual Process.
Similarly, the process can be automated when we use AWS Lambda, lambda code should be written in such a way that App-server listening on one RDS Endpoint will be pointed to the new RDS Endpoint in which the read-replica is resided. 

# STEP 12: TAKING BACKUP TO DIFFERENT REGION
We should take to automatic backup for the RDS to the same or different region, to the S3. If required, we can restore the database from that last point of your backup. Configure automated backups and snapshots to ensure point-in-time recovery and data protection. Set an appropriate retention period based on your recovery point objective (RPO) and compliance requirements.

Consider these practices as well in designing the Architecture.
1. Enable Enhanced Monitoring and Alerting Mechanism
2. Enable automated Patching
3. Properly collect logs and optimize the database if needed
4. Minimal Services should be allowed in security groups
5. Place the RDS in Private subnet

 
                

