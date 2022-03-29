# AWS-ASSOCIATE
## Known terms to be used
- EC2 - A compute instance
- Lambda - Used to provide serverless application architecture. That means that network events like consumer requests can trigger the execution of a pre-defined code-based operation
- Elastic Beanstalk - managed service by AWS. All you need to do is to push your application code and AWS does the rest for you

### Networking terms to be used
- VPC - Used to secure, isolate and configure network environments around your instances
- Direct Connect - Can be used to establish an enhanced direct tunnel between your local data center and your AWS based VPCs
- Route 53 - AWS DNS service that lets you manage domain registration
- CloudFront - Amazons distributed global CDN. When configured properly this can store cached versions of your site's content at edge locations across the world

### Storage terms to be used
- S3 - inexpensive object storage
- S3 glacier - good choice for when you need larger data archives stored cheaply over long term and can live with retrieving delay in hours
- Elastic Block Store - provides persistent virtual storage drives that host the OS and working data of an EC2 instance. Meant to mimic the storage drives and partitions attached to physical servers
- Storage Gateway - hybrid storage system that exposes AWS cloud storage as a local, on premises appliance

### DB terms to be used
- Relational Database Service - managed service that builds you a secure, stable reliable DB instance
- DynamoDB - used for fast flexible and scalable managed nonrelational (NoSql) DB workloads

### Application Management terms to be used
- Cloud watch - to monitor process performance and resource utilization and when thresholds are met to send messages and etc
- CloudFormation - enables you to use template files to define full and complex AWS deployments. Scripting basically
- CloudTrail - collects records of all your API events. 
- Config - to help with change management and compliance of your AWS account (Define a base config - if the config moves away from that. you get notified)


### Security and identity terms to be used
- Identity and Access Management (IAM) - for user access and auth to your AWS account
- Key Management Service - administer creation and use of encryption keys to secure data used by AWS resources
- Directory Service - For integration with other identity providers like AD

### Application integration terms to be used
- Simple Notification Service (SNS) - automate alerts to other services (e.g to trigger a lambda function) or SMS
- Simple Workflow - lets you coordinate a series of tasks that must be performed using AWS services or even non-digital events
- Simple Queue Service - event-driven messaging within distributed systems that can decouple while coordinating the discrete step of the larger process. The data contained in your SQS messages will be reliably delivered, adding to the fault tolerant qualities of an application
- API Gateway - to create and manage secure and reliable APIs for your AWS based applications

### Basic commands to create an S3 bucket using AWS CLI

```
aws s3 ls
aws s3 mb s3://mybucket --region us-west-1
aws s3 cp test s3://test-parth-123
aws s3 rb s3://test-parth-123 --force
```

### Provisioning an EC2 instance

An AMI is really just a template that has information telling EC2 what OS and application software to include on the root data volume

Four kinds of AMIs:
1. Amazon quick start AMIs - self
2. Marketplace AMIs - prod ready images
3. Community AMIs - indendepently created stuff
4. Private AMIs - own stuff

AMIs is only available in one region. if you invoke the ID of an AMI from a different region it will fail

There are many instance types to suit your needs. Feel free to browse them here:
aws.amazon.com/ec2/instance-types

### Configuring an environment for your instance

Three things to get right when configuring your instance:
Geographical region, VPC config and tenancy model

- A good idea to separate/create a new VPC for each of your project or project strages like different VPCs for early app dev another for beta testing etc
- Tenancy - can be shared or dedicated - shared means you could share the server with other customers and opposite for dedicated. Costs will be obvio ....



####NOTE:
You can always change an instance type as followe:
1. Stop the instance. (Remember, your public IP address might be different when you start up again.)
2. From the Actions drop-down, click Instance Settings and then Change Instance Type. Select a new type.
3. Restart the instance and confirm that it’s running properly.


### Configuring instance behaviour
You can tell EC2 to run some commands while its booting up (bootstrapping). You can specify this is in the console or using the --user-data value with the AWS cli. This is the best time to feed any scripts that you may want to.

### EC2 placement groups
This gives you the power to define non-standard profiles to place your instances to meet your needs. Three kinds:
1. Cluster groups - launch easy instance into one zone (close proximity)
2. Spread groups - separate instances physically across different H/w racks and even availability zones to reduce risk of failure. Similar to VMWare's Distributed Resource Scheduler
3. Paritition groups - lets you associate some instances with each other. Placing them in a single partition. But the instances within that single partition can be kept physically separated from instances within other partitions. This differs from spread groups where no two instances ever share physical hardware

## Terminating an instance - Things to note
This destroys the data inside but only if you are using an Elastic Block Store (EBS) and the volume is set to persist that you can keep the data (used in k8s)
- You should also be aware that a stopped instance that had been using a non-persistent public IP will now be assigned a new IP address when restarted
- If you need a predicatable IP address, allocate an elastic IP address and associate it to your instance
- You can edit an instances security policies even while its running without any issue
- You can also increase compute, storage, memory even while the instance is running

## Resource Tags
Used to identify stuff. Has a key and a value e.g: Production server - server1

## Service limits on AWS
Limits could apply like 5 VPCs per region or 5000 SSH key pairs across your account. This could be raised by AWS

# EC2 Storage Volumes
## 1. Elastic Block Store Volumes
- You can attach as many EBS volumes to your instance as you would like (only one at a time can attach tho)
Four EBS volume types:
- EBS-Provisioned IOPS(Input Output Per Second) SSD - for intense use 64000 IOPS/Volume and a maximum throughput of 1000 mb/s
- EBS-General Purpose SSD - 16000 IOPS/Volume - Costs can be checked
- Throughput Optimized HDD - for log processing and big data operations that need high throughput - 500 IOPS/volume
- Cold HDD - larger volumes of data that require infrequent access

# Instance Store Volumes
These are ephemeral - Why use instance store volumes:
- These are SSDs attached to the server hosting your instance and connected via a fast NVMe (Non-Volatile Memory Express) interface
- Use of instance volumes is included in the instance pricing
- Work well for deployment models where instances are launched to fill short-term roles (for lets say importing some stuff)

Whether one or more volumes will be available for your instance will depend on the instance type you choose

## Accessing your EC2 instance
- Out of the box your instance can only connect within the subnet to other resources 
- If your instance needs multiple network interfaces (to connect to other resources) you can create and attach one or more virtual elastic network interfaces to your instance. Each of these interfaces must be connected to a subnet and security group.
- You can also assign a static IP within the subnet range
- Default public IP assigned to your instance is always ephemeral and can be staticized by elastic IPs (no charge)
- Running the following curl command will return the types of data available about the EC2 instance from within the instance

```
[ec2-user@ip-172-31-86-152 ~]$ curl http://169.254.169.254/latest/meta-data/
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
events/
hibernation/
hostname
identity-credentials/
instance-action
instance-id
instance-life-cycle
instance-type
local-hostname
local-ipv4
mac
metrics/
network/
placement/
profile
public-hostname
public-ipv4
public-keys/
reservation-id
security-groups
services/
```
Entries containing a trailing slash contain further info. You can use that to display info like below:
```
curl http://169.254.169.254/latest/meta-data/security-groups
launch-wizard-4
```
## Securing your EC2 instance
This is what AWS provides to secure your instance:
- Security groups
- Identity and Access management
- NAT
- Key pairs

### Security Groups
- This is like a firewall. By default this denies all kind of traffic. 
- You define group behaviour by setting policy rules that will either block or allow traffic
- Traffic is assessed based on the source and the destination as well as the port and protocol its going to use
- AWS also provides Network Access Control Lists - that are associated with entire subnets rather than just instances

### IAM Roles
- When a role is assigned to a user or a resource, they'll gain access to a resource that match the role policies
- Using roles you can give limited amount of entities exclusive access to resources like EC2 instances
- You can also assign an IAM role to an instance so that it can access external tools like a Relational Database Instance

### NAT Devices
- Sometimes you need to configure an instance without a public IP address so you dont expose it to the public
- AWS gives you two ways to achieve this:
1. NAT instance
2. NAT gateway - managed service - so you dont have to manually launch and maintain an instance - both incur charges

### Key Pairs
Self explanatory

## EC2 Auto Scaling
EC2 auto scaling uses either *Launch Configuration* or *Launch Template* to automatically configure the instances that it launches
### Launch Configuration
This is a template/named doc that contains information to not basically go through the whole process of creating an instance
- You can create a launch configuration from an existing EC2 instance. Auto scaling will copy the settings but you can edit as you wish
- Launch Configs are only used for EC2 Auto Scaling - meaning you cant manually launch an instance using the launch config
- Once you create your launch config you cannot modify, need to create a new one

### Launch Templates
- You can use a launch template to auto scale but also to spin up EC2 instances or even creating a spot fleet
- They are also versioned, allowing you to change them after creation

## Auto Scaling Groups
ADD LABS WHEN YOU START
