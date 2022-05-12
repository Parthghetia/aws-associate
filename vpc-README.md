# VPC
## Default VPC Overview
- It is advisable to create your own VPC in prod and not use the default VPC that is shipped by default
- Subnets are created under the VPC and each subnet has its own IPV4 allocation from where the IP address will be fetched
- To each route table in a subnet, in some, you'll notice an internet gateway attached to it. This is the internet gateway that will allow traffic to go to the internet
- Max CIDR per VPC is 5 and for each CIDR:
1. Min Size is /28 - 16 IPs
2. Max Size is /16 - 65536 IPs

### VPC Hands On - Without the VPC Wizard
![image](https://user-images.githubusercontent.com/43883264/167526195-a12a02fe-1ae0-43a3-9507-db7a34a7ce3a.png)
- The tenancy attribute above is to differentiate betweeen shared hardware and dedicated hardware
![image](https://user-images.githubusercontent.com/43883264/167526302-4d68fd84-71f5-4282-83d6-d5ccd0b6c81b.png)
![image](https://user-images.githubusercontent.com/43883264/167526335-d0b823cd-5e68-4500-b287-7351bd4b3017.png)

### VPC Subnets (IPV4) - Reserved IPs
![image](https://user-images.githubusercontent.com/43883264/167526578-3d5a4597-3009-410a-b120-3527c807c70a.png)


### Subnet Hands On
- If you filter subnets based on the VPC id of the newest VPC created above you will see no subnet. We will create one
- In the snip below, we choose a VPC and we are going to create two subnets. Two public and two private. Public is mostly for your internet facing services (thus smaller size) like Load Balancers, or NAT Gateways etc.
![image](https://user-images.githubusercontent.com/43883264/167530776-4a9cda5a-62c3-4988-836b-16f8716be4c5.png)
- Time to now create our private subnets
![image](https://user-images.githubusercontent.com/43883264/167530926-8d923bcd-901e-4102-82ae-e50f0297140c.png)

## Internet Gateways
- Our created subnets, dont have internet access yet
- Must be created separately from a VPC. It also scales horizontally and is redundant
- One VPC can only be attached to one IGW
- IGW do not allow internet access, routing tables must be editted like so:
![image](https://user-images.githubusercontent.com/43883264/167531273-7d66daef-372a-483d-8e39-352fb7838fdc.png)

### IGW and Routing Tables - Hands On
- For testing purposes we will create an EC2 Instance and attach the VPC created above to see if it can go to internet or not
![image](https://user-images.githubusercontent.com/43883264/167532171-efe08b37-87e9-4ebb-acb0-dea6e3faa7bc.png)
- If you even try to access the instance after that. You won't be able to
![image](https://user-images.githubusercontent.com/43883264/167532433-59efa75c-ecde-4662-90c1-fb06681cc471.png)
- Now let's create an IGW and attach it to our VPC like so:
![image](https://user-images.githubusercontent.com/43883264/167532517-77ea25c7-5f85-4a6a-9473-1ff1de744647.png)
![image](https://user-images.githubusercontent.com/43883264/167532557-93eee7ab-09c5-4b0f-9e4f-5ee84cc2e2f3.png)
- and just attach to desired VPC. You still cannot reach to your instance, due to missing routes. Which we will correct below
- If you see currently, you will only notice the default route table in your VPC that has no explicit subnets associated to it. We need to assign these
![image](https://user-images.githubusercontent.com/43883264/167532936-29d43b2e-c7b9-4e54-8b4f-d759cedfc750.png)
- We create two new route tables, and we will now assign subnets explicitly
![image](https://user-images.githubusercontent.com/43883264/167533087-c1c8ba25-0bd3-49da-9286-d1af3c3f7a77.png)
![image](https://user-images.githubusercontent.com/43883264/167533180-35088cfd-8b6a-4929-93c5-fabd0106c3ad.png)
- We finally need to add a route for any IP coming from the outside should hit the internet gateway. kinda like a default route like below. Remember, we gave the instance in a public subnet, so the routes shall only apply to that public subnet. 
![image](https://user-images.githubusercontent.com/43883264/167533420-ecf024ed-d88d-4521-9fa2-d96a03c66828.png)|
- And now Voila!! You can reach your instance using SSH or whatever as per your security groups. And can reach the internet too
- In the coming sections we shall see how to provide private subnets, internet access

### Bastion Hosts
![image](https://user-images.githubusercontent.com/43883264/167533662-45b60391-902f-48bb-8392-d57358776556.png)

- So the public instance above, we will now treat it is a bastion host. And we will create an instance in the private subnet with the security groups set to hit only the security group of our public subnet
![image](https://user-images.githubusercontent.com/43883264/167707602-e21aa7af-ceed-46fd-9fc4-da3f50a30938.png)
- For this instance now, we cannot EC2 instance connect to it. So we need to have a different key pair to be able to go to the bastion host and connect to it. Don't forget to gen this before creating the instance. Tough to change later.

### NAT Instances till DNS got lost, will add it later again

## NACL and Security Groups
- NACL are stateless and Security groups are stateful. So as SGs are stateful, once an inbound rule is already allowed, the outbound rule does not make a difference and traffic for that particular flow will be still allowed
![image](https://user-images.githubusercontent.com/43883264/167911982-fa4d7aa0-810c-4a27-bc6c-356cc844f1fe.png)
- What are NACLs
![image](https://user-images.githubusercontent.com/43883264/167912309-6b78a4ad-6391-4a14-baa3-7410d13d180e.png)
![image](https://user-images.githubusercontent.com/43883264/167912471-5d578730-6f47-4628-88b6-27785959a9b0.png)

- Default NACL allows everything
- Ephemeral Ports:
   Whenever a client is trying to connect to a system it uses an ephemeral port to connect to the service like below:
   ![image](https://user-images.githubusercontent.com/43883264/167913168-7e221963-27d3-44e0-835f-f5068387af1e.png)
- NACL with Ephemeral Ports
![image](https://user-images.githubusercontent.com/43883264/167913495-6d181eec-a338-4298-8b77-3bbf291c97ba.png)
 Watch the bottom flow!!
 - Difference between security groups and NACLs
 ![image](https://user-images.githubusercontent.com/43883264/167913763-197a56b5-40c5-467e-8566-97a66025feab.png)

### NACL and Security Groups Hands On
- Pretty straightforward to be honest. Just the difference between stateful SG vs stateless NACL

## VPC Reachability Analyzer
![image](https://user-images.githubusercontent.com/43883264/167917761-7675e494-47cd-47c1-87b7-932dbe6fdfa8.png)

### VPC Reachability Analyzer - Hands On
- Costs 10 cents per analysis
![image](https://user-images.githubusercontent.com/43883264/167918208-817ffe4f-0d4b-44a6-837a-88c87f80e60b.png)
![image](https://user-images.githubusercontent.com/43883264/167918381-52319b12-0348-47ba-a70e-9bcf27735a85.png)
![image](https://user-images.githubusercontent.com/43883264/167918439-1f5820c1-2a4a-4577-8af8-0f1813c1f653.png)
![image](https://user-images.githubusercontent.com/43883264/167918534-df720970-edfd-4b51-a36d-8cf4a8fd40ec.png)

## VPC Peering
![image](https://user-images.githubusercontent.com/43883264/167919237-ce181ae1-0f59-4f7c-938e-9193e70b42cb.png)
![image](https://user-images.githubusercontent.com/43883264/167926208-f46a737a-d66a-41de-9f43-d53a590e8c1d.png)
![image](https://user-images.githubusercontent.com/43883264/167926295-6bf7fa95-a0b3-450c-a927-f124fa00935b.png)

### VPC Peering Hands On
![image](https://user-images.githubusercontent.com/43883264/167926982-ed29c632-1fc1-4fcd-be95-808bdc6b2e84.png)
- Gotta accept the pending requests
![image](https://user-images.githubusercontent.com/43883264/167927094-db8b7602-f112-4fe6-839c-5fdde1eed7cc.png)
- We need to now add routes on both VPCs to allow inter-VPC communication like so to hit the peering connection whenever going to the CIDR on the other VPC
![image](https://user-images.githubusercontent.com/43883264/167927360-9a078b4a-97a3-4ef3-9024-948dba8aa42c.png)

## VPC Endpoints
![image](https://user-images.githubusercontent.com/43883264/167969716-fd6abf24-526b-4db8-80d4-b4a46eabc192.png)
![image](https://user-images.githubusercontent.com/43883264/167970052-2d8687f4-8065-4a33-92e2-8f9846c548c3.png)
![image](https://user-images.githubusercontent.com/43883264/167970068-f7b752f3-b74c-4cf3-874c-170b80704d1d.png)

### Types of VPC Endpoints
![image](https://user-images.githubusercontent.com/43883264/167970203-2b26b6b3-1442-4ffd-b3d3-7c1fa9f262b2.png)

### VPC Endpoints Hands On
- To test this, we are going to use our private instance. It currently needs creds to reach to s3
```bash
[ec2-user@ip-10-0-19-15 ~]$ aws s3 ls
Unable to locate credentials. You can configure credentials by running "aws configure".
[ec2-user@ip-10-0-19-15 ~]$ 
```
- Lets attach an IAM role to allow S3 read only access to our EC2 instance
![image](https://user-images.githubusercontent.com/43883264/167971250-d7aec6a1-7e6a-4659-bde8-31f7d65abd85.png)
- If you try to access the s3 buckets without a NAT instance, you will not be able to reach it. Why?? Because you are in the private subnet and we haven't allowed access to the S3 Service from the outside world. And for internal access you need a VPC endpoint
- Now let's create an endpoint
![image](https://user-images.githubusercontent.com/43883264/167972545-831d5a0a-ff27-4883-9a57-0a674a346284.png)
![image](https://user-images.githubusercontent.com/43883264/167972554-dde49840-eaf6-4e76-b63e-b1f5d621ee02.png)
- And now voila!!! It works
``` bash
aws s3 ls
2022-04-30 23:25:49 demo-cloudfront-parth-v2
```

## VPC Flow Logs
![image](https://user-images.githubusercontent.com/43883264/167973293-3f8eda38-8c41-4e7b-88ac-61483de897a2.png)

### VPC Flow Logs Syntax
![image](https://user-images.githubusercontent.com/43883264/167973419-06c42335-1ef5-47c0-8d11-615a6911597e.png)
#### VPC Flow Logs Tshooting
![image](https://user-images.githubusercontent.com/43883264/167973525-26999ae8-9c9f-49b4-9b66-e0b47d297c55.png)

## VPC Flow Logs - Hands On
https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/13531264#content

## AWS Site-to-Site VPN
![image](https://user-images.githubusercontent.com/43883264/167975244-1b84654f-e4dc-4a05-bb23-9736c4a32c89.png)
![image](https://user-images.githubusercontent.com/43883264/167975382-694640d2-06d8-4077-a9ad-cc3ef283d60e.png)

#### Site-to-Site VPN Connections
![image](https://user-images.githubusercontent.com/43883264/167975619-bdbcd7db-1862-4d7d-8987-56e43fbcff44.png)
![image](https://user-images.githubusercontent.com/43883264/167975743-24a48b90-8b7e-491d-a154-19e0b5e3ef43.png)
- Hands On https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/28874574#content

## Direct Connect (DX)
![image](https://user-images.githubusercontent.com/43883264/167976422-5154d186-0160-43c1-885c-07f215322110.png)
![image](https://user-images.githubusercontent.com/43883264/167976605-985797e7-432f-4df5-b012-4810b80d69cb.png)

#### Direct Connect Gateway
![image](https://user-images.githubusercontent.com/43883264/167976709-43300860-f214-4997-bbaf-3d34b687a661.png)

#### Direct Connect - Encryption
![image](https://user-images.githubusercontent.com/43883264/167976937-12da885e-4490-44c8-88d4-e0316317f2de.png)

#### Direct Connect - Resiliency
![image](https://user-images.githubusercontent.com/43883264/167977063-8b9213bc-d452-41ad-bc78-90add1dc3591.png)

## AWS Private Link - VPC Endpoint Services - used to expose services in your VPC to other VPCs
- These are the options we have
![image](https://user-images.githubusercontent.com/43883264/167977178-80b70ecb-b8ee-4331-877f-9e9e6545cfeb.png)
![image](https://user-images.githubusercontent.com/43883264/167977324-c83ee359-5657-419a-b62b-2c9bec310de4.png)

#### AWS Private Link and ECS
![image](https://user-images.githubusercontent.com/43883264/167977440-e701df9d-14d1-4a34-88f8-45ff1d1dcf6d.png)

#### AWS Private Link VPC Endpoint Services - Hands On
![image](https://user-images.githubusercontent.com/43883264/167977663-559b54a7-d622-4751-b658-f222efdffeaa.png)
- On the other VPC, you would create an Endpoint as below and enter a private service FQDN that is exposed out
![image](https://user-images.githubusercontent.com/43883264/167977754-4a1856e0-276b-4954-9ee3-fe8eb76ece79.png)

## Transit Gateway
- Network Topologies can become complicated real fast, thus the solution is transit gateway
![image](https://user-images.githubusercontent.com/43883264/167978125-d56310e8-e15d-4e2c-942e-459ca894950f.png)
- Another use case for Transit Gateway - is to increase bandwidth for the Site-to-Site VPN using ECMP
![image](https://user-images.githubusercontent.com/43883264/167978333-31ddca3a-d115-44df-82ff-350779224503.png)

- You can also share a direct connect between multiple accounts like so:
![image](https://user-images.githubusercontent.com/43883264/167978535-d228daee-700e-48f8-8561-f32a62954c19.png)

## VPC Traffic Mirroring
![image](https://user-images.githubusercontent.com/43883264/167978772-03aa6e61-e718-4480-b2bc-fdf38ed372d8.png)

## IPv6 for VPC
![image](https://user-images.githubusercontent.com/43883264/167978977-bd26459a-f934-4b89-99e1-f08b6bac4c2b.png)

- If IPv4 IPs run out in a subnet
![image](https://user-images.githubusercontent.com/43883264/167979258-8dbf8198-d4c2-49b6-8d09-042dfb62237c.png)

Enabling IPV6 access in a VPC -  Hands On - Not so important as V4 is always always needed
https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/26099596#content

## Egress Only Internet Gateway
![image](https://user-images.githubusercontent.com/43883264/167981741-bfe59c46-ddc6-4ae3-9dba-5613132776af.png)
#### IPv6 Routing for Egress Only Internet Gateway
![image](https://user-images.githubusercontent.com/43883264/167982076-d6d1f693-fc1b-40c7-a162-e74266d511b7.png)

## VPC Summary
![image](https://user-images.githubusercontent.com/43883264/167983092-1398ed23-24fb-4c73-9d99-e481d644e0f9.png)
![image](https://user-images.githubusercontent.com/43883264/167983297-18d88d81-533f-4c68-bd8d-b4aeda3c3b8e.png)
![image](https://user-images.githubusercontent.com/43883264/167983329-970e786c-a7fa-4670-b0a0-7dd31e90a499.png)
