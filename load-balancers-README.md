- [High Availability](#high-availability)
  - [Load Balancing](#load-balancing)
  - [Classic Load Balancers](#classic-load-balancers)
  - [Application Load Balancer](#application-load-balancer)
    - [Application Load Balancer - Hands On](#application-load-balancer---hands-on)
  - [Network Load Balancer](#network-load-balancer)
    - [Network Load Balancer - Hands on](#network-load-balancer---hands-on)
    - [Gateway Load Balancer](#gateway-load-balancer)
  - [Sticky Sessions in Load Balancers](#sticky-sessions-in-load-balancers)
  - [Cross Zone Load Balancing](#cross-zone-load-balancing)
      - [How to enable cross-zone load balancing on the LB](#how-to-enable-cross-zone-load-balancing-on-the-lb)
  - [Load Balancer - SSL certificates](#load-balancer---ssl-certificates)
    - [Server Name Indication in SSL certs](#server-name-indication-in-ssl-certs)
        - [NOTE: Connection draining/Deregistration delay - this is basically the time to complete "in-flight requests while the instance is de-registering or unhealthy](#note-connection-drainingderegistration-delay---this-is-basically-the-time-to-complete-in-flight-requests-while-the-instance-is-de-registering-or-unhealthy)


# High Availability
- This in AWS means running your application/system in at least 2 DCs (- AZs)
- Vertical scaling - increase size of instance and horizontal means increasing number of instances.
- Here are the tools to use in order to achieve, availability and scalability
![image](https://user-images.githubusercontent.com/43883264/163860914-bbd041d5-aa70-4ccd-a77a-234160f9cc1d.png)

## Load Balancing
- Servers that forward traffic to multiple servers downstream
![image](https://user-images.githubusercontent.com/43883264/163861081-aca9fd3a-abcd-44de-a753-7dc8b34cafbc.png)
- Why use a load balancer?
![image](https://user-images.githubusercontent.com/43883264/163862979-f5c2b23d-d7f2-4c0c-95fb-7cb358c9083d.png)
- Why use an elastic load balancer?
![image](https://user-images.githubusercontent.com/43883264/163863352-158fc37a-d8ba-4ca8-bb79-328adf16ba65.png)
- Health checks - how they work?
![image](https://user-images.githubusercontent.com/43883264/163863596-fc091e41-d062-4531-a24a-8d56552b7310.png)
- Types of Load Balancers on AWS
![image](https://user-images.githubusercontent.com/43883264/163864675-fa866342-33e0-4436-9457-f6db763a07f9.png)
- Load balancer security groups
![image](https://user-images.githubusercontent.com/43883264/163865123-b64aaa64-bde4-4c74-9d1b-7345df7a3001.png)
Basically security groups linking together here 

## Classic Load Balancers
- An old style of LBs
![image](https://user-images.githubusercontent.com/43883264/163898185-3e3de955-390f-430e-868f-cc17373be69a.png)
- As this is deprecated. If you wanna go through the hands on. Go here: https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/26148288#content

## Application Load Balancer

![image](https://user-images.githubusercontent.com/43883264/163900462-fc0f9f34-9a2f-4ba4-8627-2b0baf57f6eb.png)

![image](https://user-images.githubusercontent.com/43883264/163900582-4997b97f-b667-4f0f-854a-3389760444e2.png)

![image](https://user-images.githubusercontent.com/43883264/163900942-72022c3c-c7d4-4dc8-963e-eaf3e5dd8f06.png)

### Application Load Balancer - Hands On
![image](https://user-images.githubusercontent.com/43883264/164094918-7bfca99e-439e-4a78-a5b2-3090c826c344.png)

- For security groups, you need to create both an incoming and outgoing security group. Think of it. LB is supposed to take in traffic and push it out. So gotta allow both right??? So everything from outside coming on port 80 (for a web server). Then anything from the LB to a security group common to all instances allowing port 80 traffic out of the LB.
- You then create a target group as below:
![image](https://user-images.githubusercontent.com/43883264/164098256-eefae306-b548-4297-b0af-62e8ecf4bfae.png)
![image](https://user-images.githubusercontent.com/43883264/164098683-20d1b356-b149-44e0-a2e4-e4c56c95fd55.png)

With ALBs you could actually program the traffic. So we created another target group, and we are going to define a flow that says. If we hit <app-dns-fqdn>/test - please go to target group XYZ. Basically we could route our test-app traffic to the instances in that target group

- So once ALB is created you can edit rules here:
![image](https://user-images.githubusercontent.com/43883264/164102719-924baedd-7cb8-4286-96e7-98c23ed3d3c1.png)
![image](https://user-images.githubusercontent.com/43883264/164102764-747451ca-705e-49a6-84d7-ebb73a48d6e7.png)

- And we could also do something like this:
![image](https://user-images.githubusercontent.com/43883264/164102844-d86147a1-2562-45fc-aa7c-fcf0b8147d52.png)

## Network Load Balancer
![image](https://user-images.githubusercontent.com/43883264/164116555-80937421-5b8a-4dba-92a1-9c5a3d81c845.png)
- Target groups that can be used for network load balancers:
  1. EC2 instances
  2. IP addresses ( must be private IP addresses on the inside)
  3. Application Load Balancer - this could be in a case where you want both HTTP level load balancing with all the rules and also use the NLB for tcp based load balancing.
  
### Network Load Balancer - Hands on
 - Its all the same there is no much difference in the hands on here. Same stuff. Just that you can assign your own Elastic IP if any within the config page
  
  ![image](https://user-images.githubusercontent.com/43883264/164117521-cce8fb37-60fd-45e2-bdce-d13a0941ee79.png)
 - Remember you always need a security group

### Gateway Load Balancer
- It's basically/mostly used for Network Virtual Security appliances like Firewalls and IDS like so
  
  ![image](https://user-images.githubusercontent.com/43883264/164118010-78fc3970-17b5-443a-bc17-f704601f58f9.png)
- Uses the GENEVE protocol on port 6081
- Target Groups could be EC2 instances or private IPs

## Sticky Sessions in Load Balancers
- This is important to know in cases where the application login gets timed out. Or a connection gets reset. This is where you need to enable this

![image](https://user-images.githubusercontent.com/43883264/164119272-dd932ed2-00b9-4d3d-a428-e2a7d91f517a.png)
  
![image](https://user-images.githubusercontent.com/43883264/164119332-f57aa1f4-83fe-44eb-84e9-2454eccf74cb.png)

- This can be changed in the target group like so:
![image](https://user-images.githubusercontent.com/43883264/164119442-8afdf9d6-431b-49d8-95a7-d28a1698a448.png)

![image](https://user-images.githubusercontent.com/43883264/164119509-a6f24cb3-47ee-4fa8-bed0-770bde8419be.png)

**How to check cookies on your browser and when it expires once it is set:**
  
![image](https://user-images.githubusercontent.com/43883264/164119646-0ade42ad-6cb7-4586-8a19-539e2b348615.png)


## Cross Zone Load Balancing
![image](https://user-images.githubusercontent.com/43883264/164120073-f3d9d807-4f68-4721-bda4-2f6a44a87344.png)
![image](https://user-images.githubusercontent.com/43883264/164120690-5989f54a-f5d9-49f1-9a35-a9699432e995.png)

#### How to enable cross-zone load balancing on the LB
![image](https://user-images.githubusercontent.com/43883264/164120742-fe532140-593e-404c-a873-568d938f2e24.png)

  
## Load Balancer - SSL certificates
![image](https://user-images.githubusercontent.com/43883264/164121753-37fd8ac8-105f-40d1-a1b3-97cb3a98a87c.png)

### Server Name Indication in SSL certs
![image](https://user-images.githubusercontent.com/43883264/164121946-996cd7be-bf9b-4ff6-ac39-f063069fe1a7.png)

- SNI solves the problem of loading multiple SSL certs onto one web-server (to serve multiple websites). This is a newer protocol that requires the client to specify the hostname of the target server in initial handshake.
- CLB supports only one SSL cert can be configured within the LB as below:
![image](https://user-images.githubusercontent.com/43883264/164122595-29f9a982-96ec-43ed-94e2-c1cce7c0497d.png)
  
- For ALBs

![image](https://user-images.githubusercontent.com/43883264/164122639-9856ad2e-0f31-4539-9ac4-5170e84c7739.png)

##### NOTE: Connection draining/Deregistration delay - this is basically the time to complete "in-flight requests while the instance is de-registering or unhealthy
