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


