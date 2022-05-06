# CloudWatch
## AWS CloudWatch Metrics
![image](https://user-images.githubusercontent.com/43883264/166616201-732e0df2-b493-4e20-8714-fc0a4bed92f1.png)
![image](https://user-images.githubusercontent.com/43883264/166616297-4a555213-66e9-487a-b19d-4575815df39d.png)

## CloudWatch Custom Metrics
![image](https://user-images.githubusercontent.com/43883264/166616613-4ae772f9-6178-417d-98af-e37630bdf2b7.png)
- Looking at this doc as a ref point, and picking a simple reference command at the end
https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-data.html
- Here is the example that was run to create a custom metric
```bash
 aws cloudwatch put-metric-data --metric-name Buffers --namespace ParthNameSpace --unit Bytes --value 231434333 --dimensions InstanceID=1-23456789,InstanceType=t2.micro
 ```
 
 ## CloudWatch Dashboards
 ![image](https://user-images.githubusercontent.com/43883264/166617025-c7c96d9a-4de1-4926-99a6-2aecf4f54efb.png)
- Hands-On - You can create an automatic dashboard easily as below
![image](https://user-images.githubusercontent.com/43883264/166617140-e5f54058-e56c-437a-8121-3798931e7219.png)
![image](https://user-images.githubusercontent.com/43883264/166617153-ef1109be-1d6e-4840-adc5-84295380f9c8.png)

#### Creating a Custom Dashboard
![image](https://user-images.githubusercontent.com/43883264/166617281-6d3b0041-45a5-4694-b967-074618656e58.png)
- Choose the stuff you want to monitor and you are good to go
![image](https://user-images.githubusercontent.com/43883264/166617297-a84a1e60-2e9e-487b-b3c6-5621ec132260.png)

## CloudWatch Logs
![image](https://user-images.githubusercontent.com/43883264/166617452-c36601c3-b312-4eed-8a81-21b33c78b0b6.png)

## Sources of CloudWatch logs
![image](https://user-images.githubusercontent.com/43883264/166617507-2b79485e-772b-4f4e-be0a-886c9b05713d.png)

### CloudWatch Logs Metric Filter and Insights
![image](https://user-images.githubusercontent.com/43883264/166617577-dddf77c8-b41a-41e7-8f6c-5668fee0c2c5.png)

### CloudWatch logs - S3 Export
![image](https://user-images.githubusercontent.com/43883264/166617635-1141f81d-f9d3-4f8b-b1dd-2e927a696e58.png)

### CloudWatch Logs Subscriptions
- These are what you apply to logs to send it to a destination like below:
![image](https://user-images.githubusercontent.com/43883264/166618080-f8aefd35-030e-4d9f-8df9-83d1cc867be3.png)

### CloudWatch Logs Aggregration - Multi-Account, Multi-Region
![image](https://user-images.githubusercontent.com/43883264/166618126-37fb6525-e6f1-467e-ad79-fa2dbe71ba20.png)

### CloudWatch logs - Hands On
- Log groups, log streams, metric filters
- Creating an alarm based on a metric filter
- Create subscription filter for different components like elasticsearch, kinesis, lambda
- Logs insights
https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/29322936#content

## Cloudwatch logs for EC2
![image](https://user-images.githubusercontent.com/43883264/166838462-db55ec80-29a7-4d5a-8481-1ded79ac923d.png)
![image](https://user-images.githubusercontent.com/43883264/166838598-c72e0e7e-1ec5-44a5-bcd5-97686ae03819.png)

## CloudWatch Alarms
![image](https://user-images.githubusercontent.com/43883264/166838974-cb99a384-d380-4624-b191-6853eb113284.png)

### CloudWatch Alarm Targets
![image](https://user-images.githubusercontent.com/43883264/166839067-94c3e165-49a0-4b3e-8f4a-a131fd756840.png)

#### CloudWatch Alarm - EC2 Instance Recovery
![image](https://user-images.githubusercontent.com/43883264/166839331-3cb423c8-1612-498f-a643-4beef1366ed4.png)

#### CloudWatch Alarms: Good to know
- How to create a test alarm
![image](https://user-images.githubusercontent.com/43883264/166839439-5214982f-84a0-4e64-9c02-c84fd851a1d3.png)

##### CloudWatch Alarms - Hands On
![image](https://user-images.githubusercontent.com/43883264/166840741-52432546-454b-4d64-97e2-fd36f30fd970.png)
![image](https://user-images.githubusercontent.com/43883264/166840786-c4fe56fa-5b40-48b3-b990-c66ca5251947.png)
![image](https://user-images.githubusercontent.com/43883264/166840896-ce33a6dd-d85f-4ad3-b01a-75149b041aa1.png)
![image](https://user-images.githubusercontent.com/43883264/166840917-8aa8e951-acd1-4c87-9593-97414fd90c5d.png)
![image](https://user-images.githubusercontent.com/43883264/166840989-f8c1e3ab-23b1-4385-9d92-33a3c864007f.png)
![image](https://user-images.githubusercontent.com/43883264/166841003-59cb66f6-c0a8-442b-8ae0-afbcc30d57a8.png)

You can then trigger a test alarm using this command to see if this is working and sending a notification to the SNS topic that you created
```bash
[cloudshell-user@ip-10-0-5-137 ~]$ aws cloudwatch set-alarm-state --alarm-name TerminateEC2InstanceOnAlarm --state-value ALARM --state-reason "Testing"
[cloudshell-user@ip-10-0-5-137 ~]$ 
```

## Amazon EventBridge
![image](https://user-images.githubusercontent.com/43883264/166844886-4e9fbeb3-56f8-427d-8f19-2aedca59a640.png)

### Amazon EventBridge - Schema Registry
![image](https://user-images.githubusercontent.com/43883264/166845242-ecfbecb5-28fd-440e-b947-0edfc727a618.png)

### Amazon EventBridge - Resource Policy - To allow events from another account/region
![image](https://user-images.githubusercontent.com/43883264/166845534-a1401191-d82f-4cef-8102-ffbb9d83cdde.png)

## Amazon EventBridge - Hands On
- Default UI - to get familiar
![image](https://user-images.githubusercontent.com/43883264/167041610-f8e1f9ed-f49b-4b4c-937b-56d985b65cdf.png)
![image](https://user-images.githubusercontent.com/43883264/167042040-deba5846-a5b6-410f-969d-7a1a2d1fd9bb.png)
- Creating an EventBus
![image](https://user-images.githubusercontent.com/43883264/167042155-f24f3a72-5eea-4c60-8a81-e91520d48e93.png)
- These are the event sources, and here are the partners and where they would be set up like Pagerduty
![image](https://user-images.githubusercontent.com/43883264/167042306-4e06ca8b-fcf7-4a61-8a9a-8a486d3426a7.png)
- So now all the events can be captured in eventBridge but you need to create some rules as below:
![image](https://user-images.githubusercontent.com/43883264/167042413-f138b521-ae1e-4024-b18e-c5820e72710d.png)
![image](https://user-images.githubusercontent.com/43883264/167042739-52fbaca4-4f7e-4d11-b7f1-c164dafd1fd8.png)
![image](https://user-images.githubusercontent.com/43883264/167042753-209d3ba3-7a7d-4e2c-95ea-d68d88489c5b.png)
![image](https://user-images.githubusercontent.com/43883264/167043452-9051c987-cd1b-44a2-966c-f05de5174a54.png)

- Schema Registry
![image](https://user-images.githubusercontent.com/43883264/167043908-93f17c24-ac08-4da8-859b-41fa69180e57.png)
![image](https://user-images.githubusercontent.com/43883264/167043931-9f7d5543-d7f6-403a-ad88-ed1ddefa7bfb.png)


# AWS CloudTrail
![image](https://user-images.githubusercontent.com/43883264/167044348-6b6b40f6-60fa-4c3d-bd28-50791942bcfd.png)
### CloudTrail High Level Overview
![image](https://user-images.githubusercontent.com/43883264/167044384-c600a75a-d119-495e-b479-6139e0cebb26.png)

### CloudTrail Events - Management and Data Events
![image](https://user-images.githubusercontent.com/43883264/167044600-3b6a71f4-2e97-4539-b942-83a87baaaf6e.png)

### CloudTrail Insights
![image](https://user-images.githubusercontent.com/43883264/167044776-3f275de4-2adc-483b-b040-348835f59fd6.png)

### CloudTrail Events Retention
![image](https://user-images.githubusercontent.com/43883264/167044852-10dc7ac7-ec4f-480a-841f-26f14c2ba3f1.png)

### CloudTrail Hands On
- UI
![image](https://user-images.githubusercontent.com/43883264/167044889-927988d8-6b43-4083-91b5-e30e45ac9026.png)
- Event History
![image](https://user-images.githubusercontent.com/43883264/167044927-74a3b4e4-b602-4063-bc4b-72e415461c3a.png)
![image](https://user-images.githubusercontent.com/43883264/167044986-942d1de9-edd3-4ec2-a4a6-991b0a442ff8.png)
![image](https://user-images.githubusercontent.com/43883264/167045026-5ea2ce46-01bd-462b-91ad-efc26aafcce0.png)

- Create a Trail to capture more events - you get an option to send logs to cloudwatch as well
![image](https://user-images.githubusercontent.com/43883264/167045647-30de9867-ce50-4b9d-9490-0d51a884e600.png)
![image](https://user-images.githubusercontent.com/43883264/167046108-0a5b8ec6-b055-4e69-8b9f-9dc6813b8747.png)

- More stuff here https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/26099064#content
- Up you'll find how to view logs in Athena, and CloudWatch which is skipped in the snaps. After all its logs!!


