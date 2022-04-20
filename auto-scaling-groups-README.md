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
