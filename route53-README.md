- [Route 53](#route-53)
  - [FQDN BREAKDOWN](#fqdn-breakdown)
    - [How DNS works - Should get basic with time](#how-dns-works---should-get-basic-with-time)
  - [Amazon Route 53 - Breakdown](#amazon-route-53---breakdown)
    - [Route 53 - Records and Record Types](#route-53---records-and-record-types)
    - [Route 53  - Hosted Zones](#route-53----hosted-zones)
    - [Route 53 - Registering a Domain](#route-53---registering-a-domain)
    - [Route 53 - Creating our first records](#route-53---creating-our-first-records)
  - [Route 53 - EC2 Setup](#route-53---ec2-setup)
  - [Route 53 - Time To Live - TTL](#route-53---time-to-live---ttl)
  - [Route 53 - CName vs Alias](#route-53---cname-vs-alias)
      - [Route 53 - Alias Records](#route-53---alias-records)
    - [CNAME Hands On](#cname-hands-on)
  - [Route 53 - Routing Policies](#route-53---routing-policies)
      - [Routing Policy - Simple](#routing-policy---simple)
      - [Routing Policies - Weighted](#routing-policies---weighted)
      - [Routing Policies - Latency-Based](#routing-policies---latency-based)
  - [Route 53 - Health Checks](#route-53---health-checks)
    - [How to monitor an endpoint using health checks](#how-to-monitor-an-endpoint-using-health-checks)
    - [Route 53 - Calculated Health Checks](#route-53---calculated-health-checks)
    - [Route 53 - Health Checks on Private Hosted Zones](#route-53---health-checks-on-private-hosted-zones)
    - [Route 53 - Health checks - Hands on](#route-53---health-checks---hands-on)
      - [Routing Policies - Failover](#routing-policies---failover)
      - [Routing Policies - Geolocation](#routing-policies---geolocation)
      - [Geoproximity Routing Policy](#geoproximity-routing-policy)
      - [Route 53 Geoproximity and traffic flow hands on](#route-53-geoproximity-and-traffic-flow-hands-on)
      - [Routing Policies - Multi Value](#routing-policies---multi-value)
      - [Using a 3rd Party Domain Registrar like GoDaddy](#using-a-3rd-party-domain-registrar-like-godaddy)


# Route 53
## FQDN BREAKDOWN
![image](https://user-images.githubusercontent.com/43883264/164948033-549220cc-a1ae-4174-9308-cc7e4a779c05.png)
### How DNS works - Should get basic with time
![image](https://user-images.githubusercontent.com/43883264/164948323-9669f777-d01f-471a-959d-806e0886669a.png)

## Amazon Route 53 - Breakdown
- Its a highly available, scalable Authoritative (The customer can update the DNS records) DNS server

### Route 53 - Records and Record Types
![image](https://user-images.githubusercontent.com/43883264/164948416-09e36b1a-144f-45f9-a847-038834870b79.png)
![image](https://user-images.githubusercontent.com/43883264/164948443-c7d257be-b1e0-41ff-9c22-d5dab1031800.png)

### Route 53  - Hosted Zones
![image](https://user-images.githubusercontent.com/43883264/164948486-b9b40402-7e99-4dde-befd-2799fdcb0880.png)
![image](https://user-images.githubusercontent.com/43883264/164948511-445e1176-6e8d-404e-8c7e-d1b81ff8e24d.png)

### Route 53 - Registering a Domain

![image](https://user-images.githubusercontent.com/43883264/164948565-b5f186ca-fa10-4bd6-8b6e-2be926f25230.png)
- Make sure to enable Privacy protection
![image](https://user-images.githubusercontent.com/43883264/164948577-e70ea7e0-6feb-4d2c-b969-0360e04ea2cf.png)
- Once the domain is created you should see a hosted zone for your domain as below:
![image](https://user-images.githubusercontent.com/43883264/164948615-004fe203-0da6-43a5-b318-ab66d62cb0c2.png)
- You should see 2 records in the hosted zone as below:
![image](https://user-images.githubusercontent.com/43883264/164948632-109c9743-556d-429b-a9f4-d41ebd0eada1.png)
- The NS server above is the one that shows which are the servers that will give authoritative answer for the domain specified


### Route 53 - Creating our first records
![image](https://user-images.githubusercontent.com/43883264/164999673-25dca4d9-2c4c-40bf-ad6c-021cbbabb8e9.png)
![image](https://user-images.githubusercontent.com/43883264/164999717-263ee2a3-33ca-4ba0-bdd2-97b69e9d1c04.png)
If you go to the cloud shell and try to dig at the record created, it should route to the IP you specified as below:

![image](https://user-images.githubusercontent.com/43883264/164999840-9a418931-d275-49be-be4e-c011ed8daad0.png)


## Route 53 - EC2 Setup
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
EC2_AVAIL_ZONE=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)
echo "<h1>Hello World from $(hostname -f) in AZ $EC2_AVAIL_ZONE </h1>" > /var/www/html/index.html
```
- In the hands on below, 3 EC2 instances in different regions were created. And the user data above, would spin up an app that returns the region details as well.
- An application load balancer is also created as shown below for the eu-central region only for all the hands on that follow
![image](https://user-images.githubusercontent.com/43883264/165000638-6ecc1426-ced8-4b2f-8311-918f49b73867.png)

## Route 53 - Time To Live - TTL
![image](https://user-images.githubusercontent.com/43883264/165000467-f69fe876-19ea-4457-add8-287c171bd88c.png)

When you do a dig, it shows in the output what is the TTL of the DNS query you did as below
![image](https://user-images.githubusercontent.com/43883264/165000546-bb589672-a463-4fd1-8be0-62ec5cb9ad1c.png)

If you even change the value, for the TTL specified, the value will still keep hitting the old DNS till it cached, until the timer runs out

## Route 53 - CName vs Alias
![image](https://user-images.githubusercontent.com/43883264/165001467-1e5c47c9-439d-4ec8-9771-ffe6aab27eb7.png)

#### Route 53 - Alias Records
- This basically maps a hostname to an AWS resource as shown in diag below:
![image](https://user-images.githubusercontent.com/43883264/165002025-b8a96dfc-a09f-4b46-882c-638a641ebc4d.png)
- However, unlike CNAME it can be used for the top node of a DNS namespace (Zone Apex) e.g example.com
- Alias record is always A/AAAA for AWS resources and you cannot set the TTL
- Alias record targets:
![image](https://user-images.githubusercontent.com/43883264/165002403-901d4d1a-22e5-4a8a-833a-262112988d4a.png)
### CNAME Hands On
![image](https://user-images.githubusercontent.com/43883264/165002430-17edb029-3b4b-4bf8-a7b6-c59469b840c3.png)
- Because we are using an ALB above we can also create an alias record for the same action as below:
![image](https://user-images.githubusercontent.com/43883264/165002517-8833bd28-5943-42fb-9fa7-789da9cb789b.png)
- Trying to create a CNAME for the Zone Apex does not work as below
![image](https://user-images.githubusercontent.com/43883264/165002590-6741d44f-8123-45ef-b2d4-51ad60548c5c.png)
- But for an alias it works:
![image](https://user-images.githubusercontent.com/43883264/165002611-0adc5500-8418-4224-9b83-7a24a88da0fe.png)


## Route 53 - Routing Policies
- DNS does not route any traffic, it only responds to queries
- Route 53 supports the following routing policies
![image](https://user-images.githubusercontent.com/43883264/165200241-4f53541d-68c8-431d-a80a-b39a66be0f5e.png)

#### Routing Policy - Simple
![image](https://user-images.githubusercontent.com/43883264/165200461-d546f49e-5b2f-4a9c-a1d6-b9355fa2dc89.png)

#### Routing Policies - Weighted
![image](https://user-images.githubusercontent.com/43883264/165200954-9cf4e44a-7a89-4639-a41a-19311c361351.png)
![image](https://user-images.githubusercontent.com/43883264/165200979-55bd4a49-052c-4a42-bb8e-d24c943ffcbc.png)
** Hands on**
![image](https://user-images.githubusercontent.com/43883264/165201098-2ba1fba2-a831-4753-b21e-fec130c83494.png)
![image](https://user-images.githubusercontent.com/43883264/165201167-e5da5ec8-280e-4077-b57a-8379679db35e.png)

#### Routing Policies - Latency-Based
![image](https://user-images.githubusercontent.com/43883264/165202996-c75fec76-79c9-435c-b988-391932b0e439.png)
![image](https://user-images.githubusercontent.com/43883264/165203116-93fe3ab4-fb93-4737-bd6b-2d8ecfa92637.png)

## Route 53 - Health Checks

![image](https://user-images.githubusercontent.com/43883264/165212453-47922539-611a-4644-8456-c7b9da4213f5.png)

### How to monitor an endpoint using health checks
![image](https://user-images.githubusercontent.com/43883264/165212782-8b1df687-a2c3-46ad-8766-81d129f55484.png)
### Route 53 - Calculated Health Checks
![image](https://user-images.githubusercontent.com/43883264/165213123-7e938df6-498c-40f3-91c4-96d3a203d0dc.png)

### Route 53 - Health Checks on Private Hosted Zones
![image](https://user-images.githubusercontent.com/43883264/165213320-6c77bd58-72f9-4a28-8397-8e9132792429.png)

Its a common use case mostly

### Route 53 - Health checks - Hands on
![image](https://user-images.githubusercontent.com/43883264/165460153-5ced866a-4ef6-461d-8b9a-8e69f8d3c61c.png)
![image](https://user-images.githubusercontent.com/43883264/165460375-8a55c749-dc2b-403c-8a5c-e59acdc75fc0.png)
![image](https://user-images.githubusercontent.com/43883264/165460674-f8ca1624-bce5-4067-8cf0-0d4df9d67253.png)
**Calculated Health Checks**
![image](https://user-images.githubusercontent.com/43883264/165460993-baec2972-1ad6-45dc-8a02-739390d7e5f5.png)
![image](https://user-images.githubusercontent.com/43883264/165461151-fe656fd0-e36e-4c59-bd0f-5209897acc4d.png)

#### Routing Policies - Failover
![image](https://user-images.githubusercontent.com/43883264/165461688-d12daebd-9c49-4910-a75e-f233025112ed.png)

![image](https://user-images.githubusercontent.com/43883264/165461855-c204ee17-d7af-415f-8ed5-71ff4745861a.png)
![image](https://user-images.githubusercontent.com/43883264/165461965-0726cb09-f463-412a-8a2e-b5020d4e065c.png)

#### Routing Policies - Geolocation
- Use cases: website localization, restrict content distribution, load balancing
![image](https://user-images.githubusercontent.com/43883264/165462646-14da6dd0-bccf-4e76-99e2-eca6cdd4b6ba.png)

![image](https://user-images.githubusercontent.com/43883264/165462945-c25dccbe-dbe8-4b8d-95a0-bdc4bdf8884b.png)

#### Geoproximity Routing Policy
![image](https://user-images.githubusercontent.com/43883264/165463552-e37547f5-f052-4515-806c-fa6ef061c9c7.png)

![image](https://user-images.githubusercontent.com/43883264/165463443-7defbbc9-6f53-4521-9910-6e27134be020.png)
![image](https://user-images.githubusercontent.com/43883264/165463939-30605a16-228a-4cb9-9a2e-713d56d35cf4.png)

#### Route 53 Geoproximity and traffic flow hands on
![image](https://user-images.githubusercontent.com/43883264/165464149-17ed84a0-b78a-4958-98d5-155ac5a3b5e3.png)
![image](https://user-images.githubusercontent.com/43883264/165464224-88ddafa5-45cf-4d49-ab1e-10ec1e73b5d2.png)

![image](https://user-images.githubusercontent.com/43883264/165464595-ba012652-5d60-4af2-a656-f35d8e8699da.png)
![image](https://user-images.githubusercontent.com/43883264/165464737-92b1e494-86cc-4c4f-8cce-7dd98f0fb675.png)
![image](https://user-images.githubusercontent.com/43883264/165464884-1a833bdb-a292-4401-8d6b-6320a4c2a588.png)
- You can then create policy records for the traffic policy created as below
![image](https://user-images.githubusercontent.com/43883264/165465198-e2cc2d85-0637-4848-bf7c-6d7a3763667e.png)
![image](https://user-images.githubusercontent.com/43883264/165465427-69dc66e0-b507-4131-a03d-fd670105e13f.png)

#### Routing Policies - Multi Value
![image](https://user-images.githubusercontent.com/43883264/165465871-cc8263a8-a51b-4a68-8a46-961510527e4f.png)
![image](https://user-images.githubusercontent.com/43883264/165466048-4a8dd7b1-8ae0-43ac-bca7-9d9626821254.png)
![image](https://user-images.githubusercontent.com/43883264/165466178-f5c7e0ea-4067-4efa-be9e-10b181ed49e5.png)

A simple dig returns all the endpoints

![image](https://user-images.githubusercontent.com/43883264/165466337-9919c2e2-9bbe-426e-a7b4-a0ab1fbfb5db.png)

#### Using a 3rd Party Domain Registrar like GoDaddy
![image](https://user-images.githubusercontent.com/43883264/165466908-f0cd4eba-e2c6-4df1-81d9-90f5e6b4e1d5.png)
![image](https://user-images.githubusercontent.com/43883264/165467076-11a1114d-6952-4488-a972-63beb448ad52.png)
