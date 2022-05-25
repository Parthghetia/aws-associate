# AWS CodeDeploy 
![image](https://user-images.githubusercontent.com/43883264/168713936-47095c18-f7b7-41b1-b817-0b795a2d9d2e.png)
- How does it work?
![image](https://user-images.githubusercontent.com/43883264/168714068-e6d2c8b4-e386-4501-bb33-97d205147f7f.png)
-> Things to know about CodeDeploy
![image](https://user-images.githubusercontent.com/43883264/168714200-0aedacf1-4443-469d-b43a-d7793cb0d4ed.png)

## AWS CodeDeploy Hands On
- As CodeDeploy does not provision resources, we need to deploy EC2 instances ourselves
- While creating the instance, we need to remember that there has to be somewhere that the instance will fetch the code and artifacts from. In our case from s3 thus an IAM role needs to be attached to the instance to allow for this. Like so:
![image](https://user-images.githubusercontent.com/43883264/169860031-dcd06673-3b04-4340-ab47-92f6a86b366e.png)
![image](https://user-images.githubusercontent.com/43883264/169860122-df31fba9-15c5-4f97-b48e-861e35b1beb0.png)

-> These are the commands that are necessary to deploy the cloudDeploy agent on your laptop
```bash
sudo yum update -y
sudo yum install -y ruby wget
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status
```
-> Let's tag our instances for use with codeDeploy later
![image](https://user-images.githubusercontent.com/43883264/169865336-03fcbe49-4429-41ac-a461-9be17639c01b.png)

-> Now let's create a CodeDeploy Application
![image](https://user-images.githubusercontent.com/43883264/169865514-33c3083c-baea-4551-8529-8c5d5fe9db7e.png)

-> We then need to create a deployment group, which is basically a set of EC2 instances. Like so
![image](https://user-images.githubusercontent.com/43883264/169865727-54a969b0-a811-407c-8a55-b865f2a1f95b.png)
- Notice how we need to create a service role for this, let's go and get this
![image](https://user-images.githubusercontent.com/43883264/169865893-eb919b5d-64b4-463e-8db8-d7bf11d67b67.png)
![image](https://user-images.githubusercontent.com/43883264/169865911-79fbfb2f-2b4a-476d-91c6-68f8c3f01acb.png)


-> Now let's continue creating our deployment group
![image](https://user-images.githubusercontent.com/43883264/169866930-280a3635-9af5-438e-ab80-6114dff5baa4.png)
![image](https://user-images.githubusercontent.com/43883264/169866953-3a1c823d-d3bb-48d8-bf28-27533ca571a5.png)

-> Now we need to create an actual deployment for this deployment group
- Before we create a deployment, we need to create an s3 bucket. This will store our CodeBuild Code, we are going to use the cli in this case to make the s3 bucket
- Here are the commands:
```bash
aws s3 mb s3://aws-devops-bucket --region us-east-1
aws s3api put-bucket-versioning --bucket aws-devops-bucket --versioning-configuration Status=Enabled --region us-east-1
```

-> I will be now pushing codeDeploy files into the s3 bucket. The files are attached in this folder under the cicd-demo folder

#### CodeDeploy - Deployment Configurations
-> Under the in place deployment type, we have some strategies to look at, let's take a look (self-expl)
![image](https://user-images.githubusercontent.com/43883264/169906104-eac6a2cc-e740-43ae-84da-3b364ec2ce73.png)

-> Taking a look at the Blue/Green deployment type, this involves taking down current EC2 instances, and spawning new ones. This also involves having an ALB, which we did not spin up in the previous section

### appspec.yml - Deep Dive
-> This is our appspec.yml
```yaml
version: 0.0
os: linux
files:
  - source: /index.html
    destination: /var/www/html/
hooks:
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root

  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root

  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root

  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root

  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
```
- This corresponds to here
![image](https://user-images.githubusercontent.com/43883264/169907513-8d0ae2ab-901d-464f-a18f-77a174ef5635.png)

#### CodeDeploy - Hooks And Environment Variables
-> Here are different types of hooks you can use in different scenarios
https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html 
Look at this section  **List of lifecycle event hooks**
-> More details follow in the doc, make sure to look at the matrix and note that anything involving block traffic needs a load balancer attached to your instances for it to work

## CodeDeploy - EventBridge Integration - Hands On
- Here is proper use case, where you want to send CodeDeploy Deployment failure notifications to slack
![image](https://user-images.githubusercontent.com/43883264/169923735-c4d0a62a-343d-49db-9e8c-dcd544c9505a.png)
![image](https://user-images.githubusercontent.com/43883264/169923863-ee5accdc-dcf8-4c35-bf01-e6dc9b6dc0e6.png)
![image](https://user-images.githubusercontent.com/43883264/169923875-1044cbef-0008-4b6b-8abf-647590a797af.png)
- You would then create a lambda function that sends this to slack
![image](https://user-images.githubusercontent.com/43883264/169923904-339de11b-496a-4b3f-8416-ab8dc6ce7493.png)

## CodeDeploy - Rollbacks
-> If you look for your deployment group, you will be able to edit it and set rollbacks.
![image](https://user-images.githubusercontent.com/43883264/169931231-8f207dbd-e15c-40e5-90ab-e6055347afbd.png)

- Rollback when alarm thresholds are met -> for this you need to create an alarm first in CloudWatch. This could be triggered based on let's say CPU Util.
![image](https://user-images.githubusercontent.com/43883264/169931615-709760f8-bb15-4c80-a30f-33d23a13a03a.png)

## Register Code Deploy on an on-premise instance
https://www.udemy.com/course/aws-certified-devops-engineer-professional-hands-on/learn/lecture/16050384#content


## CodeDeploy with AWS Lambda
![image](https://user-images.githubusercontent.com/43883264/170341250-7d21ac6c-4fb7-4e36-872d-f851574d8e3a.png)
- You would then create a new deployment group and of course a new service role for Lambda as below
![image](https://user-images.githubusercontent.com/43883264/170342199-b412fd19-a4e1-4939-a634-31baaada92de.png)
- We can also set the following deployment config in the Deployment Groups to determine how an update is rolled out
![image](https://user-images.githubusercontent.com/43883264/170342809-9421da7a-ed2d-4135-a507-967ff230b4b9.png)
- Here is a detailed breakdown on the deployment config on AWS lambda
![image](https://user-images.githubusercontent.com/43883264/170343328-ffa483cd-e763-4f39-bb4a-7e032ad1ae43.png)
- Here are some sample hooks that could be configured in the appspec.yml file in an AWS Lambda deployment on CodeDeploy
https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-lambda
