
- [AWS-ASSOCIATE](#aws-associate)
  - [Known terms to be used](#known-terms-to-be-used)
    - [Networking terms to be used](#networking-terms-to-be-used)
    - [Storage terms to be used](#storage-terms-to-be-used)
    - [DB terms to be used](#db-terms-to-be-used)
    - [Application Management terms to be used](#application-management-terms-to-be-used)
    - [Security and identity terms to be used](#security-and-identity-terms-to-be-used)
    - [Application integration terms to be used](#application-integration-terms-to-be-used)
    - [Basic commands to create an S3 bucket using AWS CLI](#basic-commands-to-create-an-s3-bucket-using-aws-cli)
    - [Provisioning an EC2 instance](#provisioning-an-ec2-instance)
    - [Configuring an environment for your instance](#configuring-an-environment-for-your-instance)
    - [Configuring instance behaviour](#configuring-instance-behaviour)
    - [EC2 placement groups](#ec2-placement-groups)
  - [Terminating an instance - Things to note](#terminating-an-instance---things-to-note)
  - [Resource Tags](#resource-tags)
  - [Service limits on AWS](#service-limits-on-aws)
- [EC2 Storage Volumes](#ec2-storage-volumes)
  - [1. Elastic Block Store Volumes](#1-elastic-block-store-volumes)
- [Instance Store Volumes](#instance-store-volumes)
  - [Accessing your EC2 instance](#accessing-your-ec2-instance)
  - [Securing your EC2 instance](#securing-your-ec2-instance)
    - [Security Groups](#security-groups)
    - [IAM Roles](#iam-roles)
    - [NAT Devices](#nat-devices)
    - [Key Pairs](#key-pairs)
  - [EC2 Auto Scaling](#ec2-auto-scaling)
    - [Launch Configuration](#launch-configuration)
    - [Launch Templates](#launch-templates)
- [LAB](#lab)
  - [Creating a launch template](#creating-a-launch-template)
  - [Auto Scaling Groups](#auto-scaling-groups)
        - [NOTE: If Min number of instances is set to 0 - auto scaling will not spawn any instances and will terminate any running instances as well](#note-if-min-number-of-instances-is-set-to-0---auto-scaling-will-not-spawn-any-instances-and-will-terminate-any-running-instances-as-well)
  - [Specifying an Application load balancer target group](#specifying-an-application-load-balancer-target-group)
  - [Health checks against application instances](#health-checks-against-application-instances)
  - [Auto Scaling Options](#auto-scaling-options)
    - [Manual Scaling](#manual-scaling)
  - [Dynamic scaling policies](#dynamic-scaling-policies)
    - [1. Simple Scaling Policies](#1-simple-scaling-policies)
    - [2. Step Scaling Policies](#2-step-scaling-policies)
    - [3. Target Tracking Policies](#3-target-tracking-policies)
  - [Scheduled Actions](#scheduled-actions)
  - [AWS Systems Manager](#aws-systems-manager)
  - [Actions](#actions)
    - [Automation](#automation)
  - [Run Command](#run-command)
    - [Session Manager](#session-manager)
    - [Patch Manager](#patch-manager)
    - [State manager](#state-manager)
    - [Insights](#insights)
    - [Built-in insights](#built-in-insights)
- [BREAKING FROM THE BOOK TO A MORE HANDS ON COURSE _ MATERIAL STAYS](#breaking-from-the-book-to-a-more-hands-on-course-_-material-stays)
  - [IAM](#iam)
  - [LAB - IAM - Creating users and groups plus adding policies](#lab---iam---creating-users-and-groups-plus-adding-policies)
    - [IAM POLICY STRUCTURE](#iam-policy-structure)
    - [IAM Policy Hands on](#iam-policy-hands-on)
    - [IAM - Password policy](#iam---password-policy)
      - [MFA setup for root account](#mfa-setup-for-root-account)
  - [NOTE: Do not use your root account to create access-keys](#note-do-not-use-your-root-account-to-create-access-keys)
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

## EC2 Instance Purchasing Options
![image](https://user-images.githubusercontent.com/43883264/162596636-bedc7d6a-73bd-46d3-bd06-7622335f78bf.png)
### EC2 On Demand
![image](https://user-images.githubusercontent.com/43883264/162596835-7511f179-a9e4-4833-9d98-b77c3c6c1420.png)
### EC2 Reserved Instances - Numbers could change with time
![image](https://user-images.githubusercontent.com/43883264/162596852-6fb8b106-93e6-4a39-837d-465e59dfdd40.png)
### EC2 Savings Plan
![image](https://user-images.githubusercontent.com/43883264/162596867-56a593c9-9ae2-4d0c-9927-98836ba2e3ad.png)
### EC2 Spot Instances
![image](https://user-images.githubusercontent.com/43883264/162596886-0de7ccf4-9616-4ea3-9e9d-8ee6ba80a07e.png)
### EC2 Dedicated Hosts
![image](https://user-images.githubusercontent.com/43883264/162596900-c5edc180-8475-4195-9572-9885001092f1.png)
### EC2 Dedicated Instances
![image](https://user-images.githubusercontent.com/43883264/162596911-827efc31-c0c3-444d-b9d1-0c65b53046b0.png)
### EC2 Capacity Reservations
![image](https://user-images.githubusercontent.com/43883264/162596953-4869c278-03e4-4b3f-b2bc-65d2f205a146.png)
### An instance purchasing guide
![image](https://user-images.githubusercontent.com/43883264/162596958-3e71d9c0-2124-4762-bb13-fd3ae5fd7e9f.png)
### Price comparison charts
![image](https://user-images.githubusercontent.com/43883264/162596967-16dc5bc6-87bc-41af-b93a-bed1248e57d7.png)

## EC2 Spot Instances - Deep Dive
- You need to define your Max Spot price, and as long as the current spot price of the instance is less than your max spot price. It keeps running. When current spot price goes higher than the max spot price, you can choose to gracefully shut down your instance over 2 mins. Once the spot price goes lower than the max price, you can run your instance
#### How to terminate spot instances
Ignore the diagram on the right - Couldn't snip it off
![image](https://user-images.githubusercontent.com/43883264/162652954-2ee1800c-e5ef-4f99-9208-0bc5f0043c96.png)

#### Spot Fleets
- Spot Fleets (Can save you alot of money)
- Are = set of spot instances + (optional) on Demand instances
- ![image](https://user-images.githubusercontent.com/43883264/162653536-97d68303-ab29-455b-9693-eeb6399050ad.png)
- The biggest benefit here, is that Spot Fleet tries to pick instances from a pool that has the least cost and thus the benefit of using it (lowestPrice)

## EC2 Instance Types
Instances are divided into 7 different types and they cover different use cases as below:
**1. General Purpose
- For diversity of workloads like web-servers or code repos. Good balance between compute, memory and networking


**2. Compute Optimized
- For compute intensive tasks like: Batch processing workloads, media transcoding, high perf web-servers, high perf computing (HPC), scientific modelling and learning, dedicated gaming servers.

**3. Memory Optimized
- For performance for workloads that process large data sets of memory
- For high perf relational/non-relational DB
- Distributed web scale cache data stores
- In memory databases optimized for Business Intelligence
- Apps performing real time processing of big unstructured data

**4. Storage Optimized
- For apps that require high, sequential, read and write access to large data sets on local storage
- For high frequency online transaction processing (OTLP) systems
- Relational and NoSql Dbs
- Data warehousing applications
- Distributed file systems

**NOTE: This is a really good website to have a nice little instance comparison for pricing hourly. Definitely worth taking a look before deciding to buy any sort of instances: https://instances.vantage.sh/ 


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
You can tell EC2 to run some commands while its booting up (bootstrapping). You can specify this is in the console or using the --user-data value with the AWS cli. This is the best time to feed any scripts that you may want to. The user-data script also runs as a root user. Here is some example of user data to run a static website just from the script code
```bash
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

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

### EC2 Instances hands-on - EC2 instances launch types
#### Spot Requests
You can always see pricing history for instances - under the spot requests tab. Here you can view pricing history for different kinds of instances like below:
![image](https://user-images.githubusercontent.com/43883264/162853642-a1ed0792-e8c0-455a-b24f-fd12dd0d20e0.png)
![image](https://user-images.githubusercontent.com/43883264/162853663-4be57eb7-2c2a-442c-84c3-acbe9002a296.png)
![image](https://user-images.githubusercontent.com/43883264/162854295-7d403361-447a-4f5c-84d3-4c6d7841a4b2.png)
- Remember spot fleets can be tricky to setup initially but save you alot of money compared to on-demand instances
- In the second snip, you can also specify based on your compute requirements as well.

### How to create spot instances - Hands on
#### Spot fleets
It's all under spot requests
![image](https://user-images.githubusercontent.com/43883264/162854228-f0795c7b-9410-4063-a2aa-e5a6159d37a8.png)
![image](https://user-images.githubusercontent.com/43883264/162854284-87f73b10-ed8f-4457-9307-c2b518f91aae.png)

#### How to create only one spot instance
Just like creating an EC2 instance but in there you gotta enable this:
![image](https://user-images.githubusercontent.com/43883264/162855280-58ce9fa4-23d9-40dd-b98c-2c0b9746b7b2.png)

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
- These can be attached to many instances, So you could have one security group for a number of instances but only in on region/vpc combination
- Its good practice to maintain one security group just for SSH access
- By default all inbound traffic is blocked and all outbound traffic is allowed
- We could also reference other security groups - basically allowing other security groups attached to other instances to allow traffic like seen in the diagram below:
- ![image](https://user-images.githubusercontent.com/43883264/162587123-44fca472-912b-4460-bbe7-ff2ecf8aa94a.png)
- In order to enable the above, you just need to choose the security group as either source or destination, depending on your requirement.

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
# LAB
## Creating a launch template
![image](https://user-images.githubusercontent.com/43883264/160720067-efb3cc87-cbb6-4c44-839c-7c334fed54ee.png)

## Auto Scaling Groups

- When creating an Auto Scaling group - you need to specify the following:
1. Launch config/template
2. How many running instances you want auto-scaling to maintain at all times
3. Min and max size of auto scaling group

##### NOTE: If Min number of instances is set to 0 - auto scaling will not spawn any instances and will terminate any running instances as well

## Specifying an Application load balancer target group
- If you want to load balance traffic to instances in your group - just plug in the name of the ALB when creating the auto scaling group. New instances will automatically be added to the ALB group

## Health checks against application instances
- By default EC2 health checks on an instance are used to determine the health of the instance - you can also integrate CloudWatch, CloudTrail health checks
- If you are using an ALB group you can also configure health checks to the application target group

## Auto Scaling Options
### Manual Scaling
As the name suggests
## Dynamic scaling policies
Auto scaling generates these metrics to ensure that EC2 instances always meet your demand:
1. Aggregrate CPU util
2. Average request count per target
3. Avg network bytes in and out (2 diff metrics)

You can always integrate new metrics from CloudWatch logs and use those. As an example, your application may generate logs that indicate how long it takes to complete a process. If the process takes too long, you could have Auto Scaling spin up new instances.

Dynamic scaling policies work by monitoring a cloud watch alarm and scaling out by increasing desired capacity. There are three dynamic policies to choose from:

### 1. Simple Scaling Policies
Whenever, metrics rises, auto scaling increases capacity. That's it. How much capacity is increased depends on the three factors (as chosen by user) below:
- ChangeInCapacity - increase by specified amount
- ExactCapacity - to an exact number as specified - e.g to 6 when load increases
- PercentChangeInCapacity - increase capacity by a percent of the current amount. 4 to 6 by 50 % (set by user)

After auto scaling completes the adjustment - it waits a cooldown period before executing the policy again, even if the alarm is still breaching (300 sec by default)

### 2. Step Scaling Policies
If the demand on your app is rapidly increasing a simple scaling policy may not do the job. With step you can add instances, based on how much the aggregrate metric has increased above the threshold.

E.g:
![image](https://user-images.githubusercontent.com/43883264/161151772-ab87fce3-df49-490b-88b1-bc16772cca58.png)

There is also a warm up time - basically time to be taken before taking into consideration the metrics of the newly added instances
### 3. Target Tracking Policies
If step scaling policies are too into the application, you could create target tracking policies. Basically, select a metric and a value and auto scaling will create a cloudWatch alarm and a scaling policy to adjust the number of instances to keep the metric near that target

Also target tracking will scale in by deleting instances based on the target metric value. Warm up time can be specified too



## Scheduled Actions
This is for proactive times before demand hits like - weekends.
When you create a scheduled action you must specify the following:
- A min, max and desired capacity value
- A start date and time

![image](https://user-images.githubusercontent.com/43883264/161168644-3e2f7f31-de26-4cb7-9ee9-9b91a2b69927.png)
![image](https://user-images.githubusercontent.com/43883264/161168671-2ac33aa6-dc7b-4311-bc1f-604674591444.png)


## AWS Systems Manager
This can handle many of the maintenance tasks. Like: upgrading and installing software, installing a new application, disabling public read from all your s3 buckets, attaching IAM instance profiles.

System Manager provides the following two capabilities:
- Actions
- Insights

## Actions
Let's you automatically or manually perform against AWS resources. These actions must be defined in *documents* divided into three:
- Automation - actions you can run against AWS resources
- Command - actions you can run against Linux or Windows resources
- Policy - defined processes for collecting inventory data from managed instances

### Automation
Stuff you can do include: restart multiple EC2 instances, update cloudFormation stacks, patch AMIs
You can either automate at once or step by step as well

## Run Command
- Systems Manager accomplishes this via an agent installed on your EC2 and on-premises managed instances
- The Systems Manager agent is installed by default on more recent Windows Server, Amazon Linux, and Ubuntu Server AMIs. You can manually install the agent on other AMIs and on-premises servers

- By default, Systems Manager doesn’t have permissions to do anything on your instances. You first need to apply an instance profile role that contains the permissions in the AmazonEC2RoleforSSM policy
- You can target instances by tag or select them individually. As with automation, you may use rate limiting to control how many instances you target at once.


### Session Manager
![image](https://user-images.githubusercontent.com/43883264/161170247-4635ed48-eb50-4466-851c-cbdbb2de5d34.png)

### Patch Manager
- Patch Manager helps you automate the patching of your Linux and Windows instances
- You can individually choose instances to patch, patch according to tags, or create a patch group. A patch group is a collection of instances with the tag key Patch Group. 
- For example, if you wanted to include some instances in the Webservers patch group, you’d assign tags to each instance with the tag key of Patch Group and the tag value of Webservers.
- Patch managers use patch baselines to decide whether the patches will be installed manually or require approval
- For Windows baselines, you can specify knowledgebase and security bulletin IDs
- For Linux baselines, you can specify Common Vulnerabilities and Exposures (CVE) IDs or full package names

### State manager
- State Manager is a configuration management tool that ensures your instances have the software you want them to have and are configured in the way you define
- Can also automatically run commands and policy documents against your instances
- To use state manager, you must create an *association* that defines the command document to run and any other params, the target instance and the schedule.
- Once you create an association, state manager will immediately run it once and then according to schedule
- AWS default association called AWS-GatherSoftwareInventory - allows collection of some info from instances - network config, file info. etc

### Insights
- They aggregrate health, compliance and operational details about AWS resources.

### Built-in insights
1. AWS Config Compliance - compliant/non-compliant with AWS Config rules. Shows brief history of config changes tracked by AWS
2. CloudTrail Events - displays each resource in the group, resource type, and last event CloudTrail recorded against the resource
3. Personal Health Dashboard - 
4. Trusted Advisor Recommendations - The AWS Trusted Advisor tool can check your AWS environment for optimizations and recommendations related to cost optimization, performance, security, and fault tolerance. It will also show you when you’ve exceeded 80 percent of your limit for a service

![image](https://user-images.githubusercontent.com/43883264/161171666-63a12ab6-2658-4664-a242-4ddcd02c56d1.png)

## IAM
- So easy stuff for creating users and groups as needed. Main thing here is policies. Basically creating custom access for users and groups. Let's say providing User Alice who is a DevOps only describe permission on EC2 instances. More to follow
- IAM is a global service and for many other services you'll have a regional approach
- Root account not advised to use - gotta create users with admin account in stead
- Inline policies are specific to let's say just one user
- 

## LAB - IAM - Creating users and groups plus adding policies
So its easy. Basically create an IAM user add it to a group, give him/her Admin privileges. Make sure to set an account alias (something readable apart from numbers) 
![image](https://user-images.githubusercontent.com/43883264/161408024-96e39750-156d-4d66-a94e-242383276d46.png)
![image](https://user-images.githubusercontent.com/43883264/161408034-96904656-1f08-4c9e-a49a-5c9746835d7c.png)
![image](https://user-images.githubusercontent.com/43883264/161408043-c81545e6-4f81-48b7-b7b5-eab224f465e1.png)
Alias changed in the right below:
![image](https://user-images.githubusercontent.com/43883264/161408055-e69a303a-44d3-4107-94a2-16ab168cf2dc.png)


### IAM POLICY STRUCTURE
- Uses basically JSON. Here's a breakdown
![image](https://user-images.githubusercontent.com/43883264/161408917-cd6a01ab-eac9-4e03-b6cb-a80f60604dd0.png)


### IAM Policy Hands on
 - The only fun thing here in this lab is that you can go to policies within IAM and get the JSON for the inbuilt policies and basically use this to create your own custom policies like so:
 ![image](https://user-images.githubusercontent.com/43883264/161889394-c95e6409-b8c0-41e9-a5ec-30a3b67d0c57.png)
- You can also use the visual editor as below to create your own policies:
![image](https://user-images.githubusercontent.com/43883264/161889786-e1578591-48f1-40ff-92d1-e937109a528f.png)

### IAM - Password policy

- Basically set a minimum password policy or require specific characters. 
- Also allow IAM users to change their password
- Prevent password re-use
- Require users to change their password after some time

-> Can be changed from account settings on AWS console

#### MFA setup for root account
- Pretty easy. Click here:
![image](https://user-images.githubusercontent.com/43883264/161890518-1da867d3-2d4c-4c8c-8622-34646350c66f.png)
- And just follow


**NOTE: Do not use your root account to create access-keys



## IAM Roles for Services

- Some AWS services will need to perform actions on your behalf and for this you need to create permissions to AWS services like to EC2 with IAM roles
### Creating an IAM role hands on
1. First you need to create a role from the IAM menu like below:
![image](https://user-images.githubusercontent.com/43883264/162265342-7ad63688-40d7-498d-b694-d3787bd5a06e.png)

2. Then choose the policies/permissions to apply to this role like read-only access
![image](https://user-images.githubusercontent.com/43883264/162265513-9bd2518f-31b3-4a88-a5ba-0fa0f6810a8f.png)
You can go as custom as possible with the permissions
THAT'S IT!

### Usage of an IAM roles in action
- Taking a perfect example - you spin up an EC2 instance - You then want to be able to see some information related to let's say IAM services on your AWS account.
- Although an AMI based Linux instance has the awscli configured into, it is a VERY BAD IDEA to login to this instance using your access key. As anyone who connects to this instance could easily retrieve this and hack the hell out of you
- Hence the way out of this is IAM roles - you basically attach an IAM role to the EC2 instance, to allow it to access a particular service. The above created role allows read-only access to IAM
- If attached to an EC2 instance you have, you will be able to view IAM info like below

![image](https://user-images.githubusercontent.com/43883264/162596403-d19bc490-bfd5-492d-a546-48aa351fd267.png)
![image](https://user-images.githubusercontent.com/43883264/162596409-032671e1-c3a1-4e1c-bf71-41fe0d46675d.png)

That's it. Now when you log back into your EC2 instance you can now view IAM related info like so:
```bash
[ec2-user@ip-172-31-93-36 ~]$ aws iam list-users
{
    "Users": [
        {
            "UserName": "parth",
            "Path": "/",
            "CreateDate": "2022-04-03T02:06:33Z",
            "UserId": "AIDAXOFC6AASKX3GZORUQ", 
            "Arn": "arn:aws:iam::511442812964:user/parth"
        }
    ]
}
```



## IAM Built-In Security Tools
![image](https://user-images.githubusercontent.com/43883264/162265884-1c668685-e046-459f-9cac-079cabd76519.png)

You can easily generate this from the IAM menu - pretty straightforward

To access the access-advisor. You gotta go to users and Access advisor like below:
![image](https://user-images.githubusercontent.com/43883264/162266736-c6965c58-82b3-44de-8937-88de3367933f.png)
- Here you can even check the policies that are used to access the policies and you could actually get rid of non-used services


## Billing in AWS
To activate the billing dashboard for a particular IAM user you need to enable IAM user and role access to billing information as below:
![image](https://user-images.githubusercontent.com/43883264/162329249-2bc1e839-9c19-4c5f-98d8-f2aa2dad0243.png)

You can now see the billing dashboard with your IAM user here
![image](https://user-images.githubusercontent.com/43883264/162329424-355934c8-22d7-423f-90b9-1e47598e6229.png)

## Creating a budget and an alert for budget limits
Get to the budget tab as below:
![image](https://user-images.githubusercontent.com/43883264/162353891-a78f12eb-f365-4589-9d64-c8bc98c9b2e9.png)

Then just create a budget and follow the options. This is what was followed by me
![image](https://user-images.githubusercontent.com/43883264/162354031-1523cafc-4b51-450e-87d0-a1b627724c1a.png)
![image](https://user-images.githubusercontent.com/43883264/162354079-dbc2143e-755b-41cb-b9b0-7c655b430c6d.png)
- You could add an action as shown below, but don't forget you'd need a role so that the service can act accordingly. Like shutting down EC2 instances
![image](https://user-images.githubusercontent.com/43883264/162354170-10fb148d-0a60-4d59-b632-383c6a7fdfc7.png)



