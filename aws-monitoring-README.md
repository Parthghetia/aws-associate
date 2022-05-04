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

