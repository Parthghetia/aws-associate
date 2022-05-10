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
