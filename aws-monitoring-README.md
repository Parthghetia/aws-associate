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




