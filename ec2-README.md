- [EC2](#ec2)
    - [EC2 instances](#ec2-instances)
  - [EC2 Instance Purchasing Options](#ec2-instance-purchasing-options)
    - [EC2 On Demand](#ec2-on-demand)
    - [EC2 Reserved Instances - Numbers could change with time](#ec2-reserved-instances---numbers-could-change-with-time)
    - [EC2 Savings Plan](#ec2-savings-plan)
    - [EC2 Spot Instances](#ec2-spot-instances)
    - [EC2 Dedicated Hosts](#ec2-dedicated-hosts)
    - [EC2 Dedicated Instances](#ec2-dedicated-instances)
    - [EC2 Capacity Reservations](#ec2-capacity-reservations)
    - [An instance purchasing guide](#an-instance-purchasing-guide)
    - [Price comparison charts](#price-comparison-charts)
  - [EC2 Spot Instances - Deep Dive](#ec2-spot-instances---deep-dive)
      - [How to terminate spot instances](#how-to-terminate-spot-instances)
      - [Spot Fleets](#spot-fleets)
  - [EC2 Instance Types](#ec2-instance-types)
    - [Configuring an environment for your instance](#configuring-an-environment-for-your-instance)
      - [NOTE:](#note)
    - [Configuring instance behaviour](#configuring-instance-behaviour)
    - [EC2 placement groups](#ec2-placement-groups)
  - [Terminating an instance - Things to note](#terminating-an-instance---things-to-note)
    - [EC2 Instances hands-on - EC2 instances launch types](#ec2-instances-hands-on---ec2-instances-launch-types)
      - [Spot Requests](#spot-requests)
    - [How to create spot instances - Hands on](#how-to-create-spot-instances---hands-on)
      - [Spot fleets](#spot-fleets-1)
      - [How to create only one spot instance](#how-to-create-only-one-spot-instance)
      - [Launching reserved instances - Discussed above](#launching-reserved-instances---discussed-above)
      - [Launching Capacity Reservations - basically i need this much compute no matter what scenario](#launching-capacity-reservations---basically-i-need-this-much-compute-no-matter-what-scenario)
  - [Resource Tags](#resource-tags)
    - [Elastic IPs on AWS - why's and why nots](#elastic-ips-on-aws---whys-and-why-nots)
    - [Elastic Network Interfaces (ENI)](#elastic-network-interfaces-eni)
    - [ENI Hands on](#eni-hands-on)
    - [EC2 Hibernate - an option to start your instances really fast - due to the following reasons](#ec2-hibernate---an-option-to-start-your-instances-really-fast---due-to-the-following-reasons)
    - [EC2 Hibernate - Hands on](#ec2-hibernate---hands-on)
    - [EC2 Optimizing CPU options for an application](#ec2-optimizing-cpu-options-for-an-application)
    - [EC2 Nitro](#ec2-nitro)
  - [Service limits on AWS](#service-limits-on-aws)
  - [AMI](#ami)
    - [Creating a Private/Custom AMI from an EC2 instance](#creating-a-privatecustom-ami-from-an-ec2-instance)
    - [Custom AMI Hands On](#custom-ami-hands-on)

# EC2
### EC2 instances
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
** 1. General Purpose
- For diversity of workloads like web-servers or code repos. Good balance between compute, memory and networking

** 2. Compute Optimized
- For compute intensive tasks like: Batch processing workloads, media transcoding, high perf web-servers, high perf computing (HPC), scientific modelling and learning, dedicated gaming servers.

** 3. Memory Optimized
- For performance for workloads that process large data sets of memory
- For high perf relational/non-relational DB
- Distributed web scale cache data stores
- In memory databases optimized for Business Intelligence
- Apps performing real time processing of big unstructured data

** 4. Storage Optimized
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

#### NOTE:
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
![image](https://user-images.githubusercontent.com/43883264/162859406-8d7704ca-65b8-42e8-a0b6-9ac90287cf50.png)
Partition Groups - will explain the rest too
![image](https://user-images.githubusercontent.com/43883264/162862205-54637f6d-438d-4dad-a88a-a4ec2f9b97fe.png)

- Creating a placement group is simple, just go to Network and Security, create group and choose one of 3 options above
- The instance then gets placed like so: 
- ![image](https://user-images.githubusercontent.com/43883264/162862096-0991a0aa-15e9-4971-b31c-520cc9c215b7.png)

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

#### Launching reserved instances - Discussed above
![image](https://user-images.githubusercontent.com/43883264/162855502-59a6a749-e2fb-4646-b507-4924093090df.png)

#### Launching Capacity Reservations - basically i need this much compute no matter what scenario
## Resource Tags
Used to identify stuff. Has a key and a value e.g: Production server - server1
![image](https://user-images.githubusercontent.com/43883264/162855817-53c64ad9-2548-4394-a3a7-b6c77f1cd396.png)

### Elastic IPs on AWS - why's and why nots
A public static IP address assigned by yourself to your instance and is totally yours till you use it. Not a really good idea to use it because of the reasons below;
![image](https://user-images.githubusercontent.com/43883264/162856750-c799b669-0638-4d1f-aa72-977b1d8d3428.png)
Hands on is basically create the IP and associate it, you can figure it. This IP can either be attached to an instance or to an interface

**DON'T FORGET TO DISASSOCIATE THE ELASTIC IP ONCE DONE AND RELEASE THE IP, OTHERWISE YOU WILL BE CHARGED FOR IT

### Elastic Network Interfaces (ENI) 
![image](https://user-images.githubusercontent.com/43883264/163076791-bfae43a4-c662-4445-bb82-460797983fa2.png)

### ENI Hands on
- Why would you need an ENI. This interface could easily be moved across instances. Amazing right!
- Imagine you got an app on a server and the server fails you could just move the IP to another server that has the same IP and your app would still keep running
- How to create an ENI
![image](https://user-images.githubusercontent.com/43883264/163078118-3df4ee43-deec-4109-a907-51ec237bf925.png)
- Make sure to choose the same subnet as the instance you want to attach to
![image](https://user-images.githubusercontent.com/43883264/163078191-32bb6a7a-1eb7-4df4-9455-b0949fab4dd4.png)

### EC2 Hibernate - an option to start your instances really fast - due to the following reasons
![image](https://user-images.githubusercontent.com/43883264/163080767-92b43c8d-91b2-4ec3-89e9-7275ac1df696.png)
- Instance RAM size must be less than 150Gb
- Cannot be hibernated not more than 60 days
### EC2 Hibernate - Hands on
![image](https://user-images.githubusercontent.com/43883264/163083972-fcc1b83e-5ac0-4542-8b67-e03acaae054a.png)
![image](https://user-images.githubusercontent.com/43883264/163084032-0a562f3f-2e1e-4fff-a522-2170cd325dbc.png)

### EC2 Optimizing CPU options for an application
![image](https://user-images.githubusercontent.com/43883264/163085454-82ffe25d-7edd-45b6-9d5e-27c44ededc67.png)

### EC2 Nitro
![image](https://user-images.githubusercontent.com/43883264/163085089-9ab3b0e7-4ef9-4a4d-9721-af091f968d44.png)
 
A good check is check uptime before and after hibernating. The same!
Make sure 
When instances are terminated - ENIs created manually will stay and the instance created ENIs will be deleted! Okay?
How you would move it from one instance to another is by just detaching from same UI above and attaching to the instance you want
## Service limits on AWS
Limits could apply like 5 VPCs per region or 5000 SSH key pairs across your account. This could be raised by AWS
## AMI

An AMI is a customization of an EC2 instance - faster boot time is what we run for, all pre-packaged. Can be built in one region and can be copied across regions

Four kinds of AMIs:
1. Amazon quick start AMIs - self
2. Marketplace AMIs - prod ready images
3. Community AMIs - indendepently created stuff
4. Private AMIs - own stuff

AMIs is only available in one region. if you invoke the ID of an AMI from a different region it will fail

### Creating a Private/Custom AMI from an EC2 instance
![image](https://user-images.githubusercontent.com/43883264/163092704-1b4b4d68-f63e-4851-8169-c57afb22a803.png)

### Custom AMI Hands On
![image](https://user-images.githubusercontent.com/43883264/163629561-5c11d04e-958a-4222-82db-732c8df83f83.png)
![image](https://user-images.githubusercontent.com/43883264/163629630-4a923a67-a662-4527-80ad-4ca5cf638f0b.png)

You can then launch instances using your custom AMI. Saves time
