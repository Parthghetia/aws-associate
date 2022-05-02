- [EC2 Storage Volumes](#ec2-storage-volumes)
  - [1. Elastic Block Store Volumes](#1-elastic-block-store-volumes)
    - [Hands on creating an EBS Volume](#hands-on-creating-an-ebs-volume)
  - [EBS Snapshots](#ebs-snapshots)
    - [EBS Snapshots Hands On](#ebs-snapshots-hands-on)
    - [EBS Encryption](#ebs-encryption)
    - [EBS Encryption Hands-On](#ebs-encryption-hands-on)
  - [2. EC2 Instance Store Volumes](#2-ec2-instance-store-volumes)
  - [3. Elastic File System (EFS)](#3-elastic-file-system-efs)
    - [EFS Performance and Storage Classes](#efs-performance-and-storage-classes)
      - [EFS Storage Classes](#efs-storage-classes)
    - [EFS - Hands On](#efs---hands-on)
    - [EBS vs EFS summary](#ebs-vs-efs-summary)


# EC2 Storage Volumes
## 1. Elastic Block Store Volumes
- A network drive you could attach to your instances. Allows persistent data even after instance termination
- You can attach as many EBS volumes to your instance as you would like (only one at a time can attach tho)


- EBS volumes things to note:
1. A network drive - so some latency is there
2. ![image](https://user-images.githubusercontent.com/43883264/163088606-842807dd-fc79-439e-a75d-36d975112284.png)
3. Multi-attach is possible
4. EBS volumes bound to an AZ like EC2 instances
5. ![image](https://user-images.githubusercontent.com/43883264/163088905-f48d78b9-8942-4d7b-b4ab-5b466a084807.png)
### Hands on creating an EBS Volume
![image](https://user-images.githubusercontent.com/43883264/163089517-85b1c54f-1a18-45c9-8492-70bd2c8a69ee.png)
- attaching the volume to an instance
![image](https://user-images.githubusercontent.com/43883264/163089621-780b1215-839f-4fd0-ab09-df2fae380628.png)

## EBS Snapshots
![image](https://user-images.githubusercontent.com/43883264/163090773-175aca8c-96be-4a02-8ced-5af723204b6d.png)

### EBS Snapshots Hands On
- Creating a snapshot is easy - just go to the volume and click on create snapshot
- Now comes the power, where you can move the snapshot to another region like so:
![image](https://user-images.githubusercontent.com/43883264/163091544-20d1b653-9060-4976-b52a-97327d8b1b80.png)
![image](https://user-images.githubusercontent.com/43883264/163091561-4c12220b-3455-4d2b-bd56-4ff76f454c59.png)

- You can then create a volume from the snapshot like so
![image](https://user-images.githubusercontent.com/43883264/163091652-ccab1916-66f8-4a10-94b2-66887ca54fe3.png)
![image](https://user-images.githubusercontent.com/43883264/163091716-18476523-02ad-4ded-99c3-c548bb463775.png)

- You also have a recycle bin as follows
![image](https://user-images.githubusercontent.com/43883264/163091857-8a6040de-96b8-465a-bebb-897d2cbaeceb.png)
![image](https://user-images.githubusercontent.com/43883264/163091905-2692c591-e6c1-4046-b33e-6b3267ad664f.png)

- You can also archive a snapshot to save money as well
EBS Snapshot features
![image](https://user-images.githubusercontent.com/43883264/163090915-b468d0d7-3a6f-4228-8c46-b0970b9cb81e.png)

Four EBS volume types:
- EBS-Provisioned IOPS(Input Output Per Second) SSD (called io1/io2 SSD) - for intense use 64000 IOPS/Volume and a maximum throughput of 1000 mb/s. io2 volumes have multi-attach capabilities to multiple instances at the same time
- EBS-General Purpose SSD (called gp2/gp3 SSD) - 16000 IOPS/Volume - Costs can be checked
- Throughput Optimized HDD (called st1 HDD)- for log processing and big data operations that need high throughput - 500 IOPS/volume -  good for Big Data, Data Warehouses, Log processing
- Cold HDD (called sc1 HDD) - larger volumes of data that require infrequent access

Details on AWS documentation for each of the storage intances above

### EBS Encryption
- All done by AWS and you dont need to do much to encrypt the data. Minimal latency diffs.
- Snapshots of encrypted volumes are encrypted 
- You can also encrypt unencrypted volumes
- Copying an unencrypted snapshot enables encryption. Yes you heard right! Copying it
- ![image](https://user-images.githubusercontent.com/43883264/163632036-b61ba2b2-9278-4956-ad42-c986d9a26a1d.png)

### EBS Encryption Hands-On
1. Create a snapshot of the volume you want to encrypt
2. ![image](https://user-images.githubusercontent.com/43883264/163632189-dd84ca5d-7f8e-4209-ad56-01707669ba23.png)
3. ![image](https://user-images.githubusercontent.com/43883264/163632216-505c41b6-cc38-40b0-ad17-d2a354d762ba.png)
4. Then create a volume from the snapshot
That's it

There is a shorter way though. Once you have a snapshot of the volume. You can create a volume from the snapshot and choose encryption. Much easier right? See below:
![image](https://user-images.githubusercontent.com/43883264/163632378-26c2ee16-1f97-40e1-8ecd-e99f8796b52c.png)
![image](https://user-images.githubusercontent.com/43883264/163632409-37a2384c-0088-4a2f-9e41-a69389a68d52.png)


## 2. EC2 Instance Store Volumes
These are ephemeral - Why use instance store volumes:
- These are SSDs attached to the server hosting your instance and connected via a fast NVMe (Non-Volatile Memory Express) interface - basically hardware attached to your EC2 instance
- Use of instance volumes is included in the instance pricing
- Work well for deployment models where instances are launched to fill short-term roles (for lets say importing some stuff)
- They are a great choice when you want great I/O performance
- Only gp2/gp3 and io1 and io2 can be used as boot volumes for EC2 instances. KEEP IN MIND!

Whether one or more volumes will be available for your instance will depend on the instance type you choose

## 3. Elastic File System (EFS)
![image](https://user-images.githubusercontent.com/43883264/163650304-828cc8c7-f1eb-43e4-bb87-ba64afb3dde7.png)
Use Cases:
- Content mgmt, web serving, data sharing, Wordpress

- It uses the NFSv4.1 protocol
- Uses security groups to control access to EFS
- Compatible with only Linux based AMIs and not windows
- File system scales automatically, and its pay per use for each Gb of storage used

### EFS Performance and Storage Classes
![image](https://user-images.githubusercontent.com/43883264/163650730-6e1e1cd9-3cc7-4315-9a2c-6e6427ab5a68.png)

#### EFS Storage Classes
- Basically used for cost saving in prod, for files that are not accessed so frequently
![image](https://user-images.githubusercontent.com/43883264/163650818-d721545e-49cb-4d3e-be4e-4ee1cc3a8531.png)

### EFS - Hands On
- Its a completely different service
![image](https://user-images.githubusercontent.com/43883264/163739451-b3200a01-a179-4a20-884e-789cd125a957.png)
![image](https://user-images.githubusercontent.com/43883264/163739485-6d0309d1-11c1-4090-913d-27bbccf8bc8f.png)
- Looking at lifecycle mgmt below, you can choose to transfer a file that has not been accessed for let's say 30 days like below to the IA storage class (to save costs obvio)
![image](https://user-images.githubusercontent.com/43883264/163739566-2c8fcda0-f8f1-48a0-a5dc-83f373607e0b.png)
- You can choose for your EFS to have accessibility to various AZs like below (Notice you can attach a SG)
![image](https://user-images.githubusercontent.com/43883264/163739901-b283baf2-7fd6-4b7e-8d8e-84586061dd8c.png)
![image](https://user-images.githubusercontent.com/43883264/163739930-caae9634-9d57-4702-84b6-905d3c36b03e.png)

Once EFS is setup, you could manually set it up on the instances by first installing the nfs-utils package as shown below:
https://docs.aws.amazon.com/efs/latest/ug/installing-amazon-efs-utils.html

You then need to mount your filesystem using the EFS mount helper on your instances like below, details to that can be find on your EFS instance when you click attach
![image](https://user-images.githubusercontent.com/43883264/163747583-218d48cd-d4d4-42b1-b802-086725f8aa8d.png)

When you try this for the first time it may time out (Yes you guessed it right! Security groups!)
An easier way to tackle this is - make one security group for all your instances (Say ec2-to-efs) - create another security group that attaches to your EFS instance (in my case created earlier) and allow port for NFS. In this, only allow the security group created earlier (in the inbound rule) and VOILA!
![image](https://user-images.githubusercontent.com/43883264/163747847-d7f7e956-baff-4d16-bcb6-02d69c5311df.png)

The instance security group basically acts as an identifier here for the EFS security group to connect to
Here is my example:

### EBS vs EFS summary
![image](https://user-images.githubusercontent.com/43883264/163859139-4c5b5636-953a-4108-8272-0e3b716a3d62.png)
![image](https://user-images.githubusercontent.com/43883264/163859667-356e106a-cb77-4855-9c3e-a204facc5088.png)

## Storage - All AWS Options - Summary
- Reference - aws-extras - for other AWS storage options
![image](https://user-images.githubusercontent.com/43883264/166176668-e4639672-8083-4146-b8dc-cd7b63651eaf.png)

