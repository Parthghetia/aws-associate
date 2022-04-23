# RDS
### What is RDS?
![image](https://user-images.githubusercontent.com/43883264/164576619-f115f26a-69fc-4006-bc12-fe831187072a.png)
### Advantages of using RDS versus deploying your own database:
![image](https://user-images.githubusercontent.com/43883264/164576729-06964fc8-66de-4cb1-b8e0-5d6a183e588d.png)
### RDS Backups
![image](https://user-images.githubusercontent.com/43883264/164576799-9c9ea76c-13a3-40da-bfe2-c2da2af53214.png)
### RDS - Storage Auto Scaling
- Just like the name suggests
![image](https://user-images.githubusercontent.com/43883264/164576997-e7ed1077-35af-4582-b926-6372d52717b5.png)

### RDS Read Replicas for Read Scalability

![image](https://user-images.githubusercontent.com/43883264/164578015-dc99e915-2862-4de4-b862-50cc22ddd9c6.png)

#### Use Cases for Read Replicas
![image](https://user-images.githubusercontent.com/43883264/164578229-35fef3d6-5a83-4f3c-b830-817a4c4b4e0a.png)

### RDS Read Replicas -Network Cost
Cross AZ but similar region replication is free but different region has the same network cost for data transfer as other inter-region services

### RDS Multi AZ (Disaster Recovery)
![image](https://user-images.githubusercontent.com/43883264/164578922-b63c8f1f-1dc9-4811-83eb-61b9b372f52c.png)

### RDS - Going from single AZ to Multi AZ
![image](https://user-images.githubusercontent.com/43883264/164579050-d1e69195-aa5f-4cda-a500-c16e4cc1c7b6.png)

### RDS - Hands on
![image](https://user-images.githubusercontent.com/43883264/164579613-0b3d682c-9880-40a3-8d77-c9dfb4b2bf6d.png)
![image](https://user-images.githubusercontent.com/43883264/164579630-28e18786-9187-4728-8bc7-e02f34425404.png)
![image](https://user-images.githubusercontent.com/43883264/164579839-b9e4a22f-81a5-4770-94ce-09fef3c920cf.png)
![image](https://user-images.githubusercontent.com/43883264/164579880-ccbb843d-9c68-4423-b800-048731b9c4ac.png)

**IN ADDITIONAL CONFIG**
![image](https://user-images.githubusercontent.com/43883264/164579934-e71d5d56-0da8-449c-82d9-5e4bbb906101.png)
![image](https://user-images.githubusercontent.com/43883264/164579958-2f375b6c-4520-40f7-8a8b-a4bf8cd430bc.png)

## RDS Security and Encryption
#### AT REST ENCRYPTION
- Encryption is defined at launch time and if the master is not encrypted, the read replicas cannot be encrypted.
#### IN-FLIGHT ENCRYPTION
![image](https://user-images.githubusercontent.com/43883264/164582850-7d3a5385-ec4d-4fd6-8f54-e24367c52951.png)
#### How to encrypt and un-encrypted DB
![image](https://user-images.githubusercontent.com/43883264/164583017-1db0af20-b52d-48ce-b037-e0283fea0625.png)

## RDS - IAM Authentication
![image](https://user-images.githubusercontent.com/43883264/164586047-9184ecd1-852e-40d2-94e9-3d6921213910.png)


# Amazon Aurora
![image](https://user-images.githubusercontent.com/43883264/164586418-a8118d2e-9764-4c6c-880a-a191c751a8d0.png)
### Aurora High Availability (HA) and Read Scaling
![image](https://user-images.githubusercontent.com/43883264/164586709-812e09e0-3f09-4ee9-8ff3-36d0544c8635.png)

### Aurora DB cluster - High Level overview
![image](https://user-images.githubusercontent.com/43883264/164586968-291296ba-6433-4c0d-bcfa-9321fa4b2eda.png)

**AURORA SECURITY IS SAME AS RDS

## Aurora Hands On
https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/13528162#content

## Aurora Advanced Concepts
#### Aurora Custom Endpoints
![image](https://user-images.githubusercontent.com/43883264/164947328-8b05a996-a3f1-4d86-87a4-ce196edc9a26.png)\

#### Aurora Serverless
![image](https://user-images.githubusercontent.com/43883264/164947346-7b110e74-7bfa-4f4e-8ca9-7f14d336c9be.png)

#### Aurora Multi-Master
![image](https://user-images.githubusercontent.com/43883264/164947363-930b1e6b-b605-4781-9d87-92a3a22ef6f9.png)

#### Global Auroras
![image](https://user-images.githubusercontent.com/43883264/164947391-6b473a56-e9d3-454b-9f7a-49a8cc1a686d.png)

#### Aurora Machine Learning
![image](https://user-images.githubusercontent.com/43883264/164947423-69839407-f835-4586-a167-e6279af22c2b.png)


# Amazon ElastiCache
- ElastiCache is used to get managed Redis or Memcached. Caches are in-memory DBs with really high perf and low latency.
- A cache will help reduce load of the DB for read intensive workloads
- Idea is that normal repetitive queries in a DB are stored here and the DB is not queried for all the queries. Only newer queries would go to the DB
- This helps make your app stateless
- ElastiCache needs alot of change in the application code to have this enabled
### Solution Architecture around using ElastiCache - Database Cache
![image](https://user-images.githubusercontent.com/43883264/164947535-cce44c74-98ee-479b-8c8c-80d77d01de93.png)

### Solution Architecture around using ElastiCache - User Session Store
![image](https://user-images.githubusercontent.com/43883264/164947578-9ea309e0-a164-43cf-8d13-1bcebb9169a2.png)
##### Side Note - Redis vs Memcached
![image](https://user-images.githubusercontent.com/43883264/164947636-71f0d004-15ee-491f-8089-db04281dfe07.png)
Basically Memcached is used when you dont care much about your data but need speed, but with Redis you really do care about storing your data

### ElasticCache Hands On
![image](https://user-images.githubusercontent.com/43883264/164947804-3f33ccf7-895a-4741-b73a-f9b36303a108.png)
- Encryption was disabled when creating it, but just to show all the options we have i enabled it in the snap
![image](https://user-images.githubusercontent.com/43883264/164947819-cbafa7bb-02e6-41d6-acff-dc68105f60e3.png)
![image](https://user-images.githubusercontent.com/43883264/164947825-a23aff6a-0499-4321-9633-6f5c578d867b.png)


### ElastiCache Cache Security
![image](https://user-images.githubusercontent.com/43883264/164947861-6617d989-65d8-49f6-9038-71148f951785.png)

### Patterns for ElastiCache
![image](https://user-images.githubusercontent.com/43883264/164947894-ef733af8-898d-4cf4-9d21-a26b7e32d321.png)

### ElastiCache - Redis Use Case
![image](https://user-images.githubusercontent.com/43883264/164947937-01c78205-ca98-43b5-afb7-e4d133b132e9.png)
