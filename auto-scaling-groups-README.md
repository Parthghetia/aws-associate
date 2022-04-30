- [LAB](#lab)
  - [Creating a launch template](#creating-a-launch-template)
  - [Auto Scaling Groups](#auto-scaling-groups)
        - [NOTE: If Min number of instances is set to 0 - auto scaling will not spawn any instances and will terminate any running instances as well](#note-if-min-number-of-instances-is-set-to-0---auto-scaling-will-not-spawn-any-instances-and-will-terminate-any-running-instances-as-well)
  - [Auto Scaling Groups - Things to know](#auto-scaling-groups---things-to-know)
    - [Auto Scaling Groups - Hands on](#auto-scaling-groups---hands-on)
  - [Auto Scaling Options](#auto-scaling-options)
    - [Manual Scaling](#manual-scaling)
  - [Dynamic scaling policies](#dynamic-scaling-policies)
  - [Predictive Scaling](#predictive-scaling)
    - [Scaling Cooldown](#scaling-cooldown)
  - [Scaling policies - Hands on](#scaling-policies---hands-on)
  - [ASG Termination Policy](#asg-termination-policy)


## EC2 Auto Scaling
EC2 auto scaling uses either *Launch Configuration* or *Launch Template* to automatically configure the instances that it launches

### Launch Configuration
This is a template that contains information to not basically go through the whole process of creating an instance
- You can create a launch configuration from an existing EC2 instance. Auto scaling will copy the settings but you can edit as you wish
- Launch Configs are only used for EC2 Auto Scaling - meaning you can't manually launch an instance using the launch config
- Once you create your launch config you cannot modify, need to create a new one
-> No need of a hands on pretty straightforward like creating EC2 instances

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

## Auto Scaling Groups - Things to know
![image](https://user-images.githubusercontent.com/43883264/164124760-2d03a7da-0259-4708-bc94-3040e40266cc.png)

### Auto Scaling Groups - Hands on
- You first need to create a launch template - pretty straightforward as you already know it
- You can then choose the type of instances that you wanna spin up and adjust the launch strategy overriding the launch template completely like so:
![image](https://user-images.githubusercontent.com/43883264/164196348-7965fbf6-919f-4612-9a08-bc35d40f7703.png)
- Below you could choose whether to attach an existing load balancer or not. You need the load balancer created first to start with and attached to a target group. The instance will automatically be added to the target group.
- Below you would choose the scaling policies - more to come
![image](https://user-images.githubusercontent.com/43883264/164200593-d1807afb-bacd-437a-8fee-6a50717737b7.png)
- If you scale the group up now. You will see that the application now load balances across instances. Cool stuff no?

## Auto Scaling Options
### Manual Scaling
As the name suggests
## Dynamic scaling policies
Auto scaling generates these metrics to ensure that EC2 instances always meet your demand:
1. Aggregrate CPU util
2. Average request count per target on the EC2 instance
3. Avg network bytes in and out (2 diff metrics)

You can always integrate new metrics from CloudWatch logs and use those. As an example, your application may generate logs that indicate how long it takes to complete a process. If the process takes too long, you could have Auto Scaling spin up new instances.

Dynamic scaling policies work by monitoring a cloud watch alarm and scaling out by increasing desired capacity. There are three dynamic policies to choose from:
![image](https://user-images.githubusercontent.com/43883264/164202701-688d59ae-7042-4d84-b7f7-60273a7782c1.png)

## Predictive Scaling
![image](https://user-images.githubusercontent.com/43883264/164203113-86772a47-ecfd-47f5-8895-403b6c1b0744.png)

### Scaling Cooldown
- After any scaling happens there is a default cooldown (no activity happens to stabilize metrics) period of 300 secs. 
- To save on config time. Best use ready-to-use AMIs to serve requests faster.

## Scaling policies - Hands on
![image](https://user-images.githubusercontent.com/43883264/164204093-ee181a2d-d075-4a6f-bcc6-8f8335c7a582.png)
- Scheduled Action
![image](https://user-images.githubusercontent.com/43883264/164204268-2527ad2f-e0de-47b4-aa7e-63bb95888bf6.png)
- Predictive Scaling Policy
![image](https://user-images.githubusercontent.com/43883264/164204478-000054cd-6dcf-430e-9f7e-d0cfe596620a.png)
- Dynamic Scaling Policies - Cloud watch alarms created for you. Can be seen as in snippet below
![image](https://user-images.githubusercontent.com/43883264/164205256-41ddbeac-550f-4692-ac91-9b3c7f516bfc.png)
![image](https://user-images.githubusercontent.com/43883264/164205688-815007f7-f736-48c0-870b-2222ab4b44f0.png)

- For step scaling and simple scaling you'd need to create your own CloudWatch alarm
![image](https://user-images.githubusercontent.com/43883264/164205418-cf809e63-44a8-401b-8419-2631b3f8a40f.png)
## ASG Termination Policy
![image](https://user-images.githubusercontent.com/43883264/164206403-95584d72-4e04-4652-8903-0f36628d1a1b.png)

In order to test this. You could try the dynamic one. install stress test utility for Amazon Linux and let it run. EC2 instances will be scaled.



