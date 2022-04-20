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
