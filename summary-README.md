
- [AWS-ASSOCIATE](#aws-associate)
  - [Known terms to be used](#known-terms-to-be-used)
    - [Networking terms to be used](#networking-terms-to-be-used)
    - [Storage terms to be used](#storage-terms-to-be-used)
    - [DB terms to be used](#db-terms-to-be-used)
    - [Application Management terms to be used](#application-management-terms-to-be-used)
    - [Security and identity terms to be used](#security-and-identity-terms-to-be-used)
    - [Application integration terms to be used](#application-integration-terms-to-be-used)
  - [Accessing your EC2 instance](#accessing-your-ec2-instance)
  - [Securing your EC2 instance](#securing-your-ec2-instance)
    - [Security Groups](#security-groups)
    - [IAM Roles](#iam-roles)
    - [NAT Devices](#nat-devices)
    - [Key Pairs](#key-pairs)
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
  - [Billing in AWS](#billing-in-aws)
  - [Creating a budget and an alert for budget limits](#creating-a-budget-and-an-alert-for-budget-limits)
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
# AWS-ASSOCIATE
## Known terms to be used
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
3. Min and max size of auto scaling group (scaling policies)
4. Load Balancer Information

![image](https://user-images.githubusercontent.com/43883264/164123945-7363eb64-bd90-4343-8a26-faacabbcda4c.png)

This is how to setup Auto Scaling Custom Metrics
![image](https://user-images.githubusercontent.com/43883264/164124045-0d9e8da3-e79d-4305-b6e3-242ada21413d.png)
##### NOTE: If Min number of instances is set to 0 - auto scaling will not spawn any instances and will terminate any running instances as well

## Specifying an Application load balancer target group
- If you want to load balance traffic to instances in your group - just plug in the name of the ALB when creating the auto scaling group. New instances will automatically be added to the ALB group

## Health checks against application instances
- By default EC2 health checks on an instance are used to determine the health of the instance - you can also integrate CloudWatch, CloudTrail health checks. 

- If you are using an ALB group you can also configure health checks to the application target group

## Auto Scaling Groups - Things to know
![image](https://user-images.githubusercontent.com/43883264/164124760-2d03a7da-0259-4708-bc94-3040e40266cc.png)

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


