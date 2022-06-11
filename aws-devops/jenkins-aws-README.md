# Jenkins
![image](https://user-images.githubusercontent.com/43883264/173204711-9fbe6d4a-5b7f-4b4c-8006-2c6b35bfa55b.png)

## Jenkins on AWS
![image](https://user-images.githubusercontent.com/43883264/173204749-2ed4507a-50d6-445b-a3cc-736aa463b069.png)

- Multi AZ Jenkins Setup
![image](https://user-images.githubusercontent.com/43883264/173204769-750df3af-688c-4640-acf9-fe8334554c8d.png)

- Setting up Jenkins on AWS
https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/
https://aws.amazon.com/blogs/devops/setting-up-a-ci-cd-pipeline-by-integrating-jenkins-with-aws-codebuild-and-aws-codedeploy/

- You can also use Jenkins with CodePipeline or with ECS
https://aws.amazon.com/blogs/devops/setting-up-a-ci-cd-pipeline-by-integrating-jenkins-with-aws-codebuild-and-aws-codedeploy/

- Jenkins works with CloudFormation, Dynamo and all other components using the right plugins

## Setting up Jenkins on AWS
- On this instance we are going to install Jenkins as both master and slave
- This is what you need to configure on the AWS instance
```bash
#!/bin/bash
# setup Jenkins on EC2
sudo yum update -y
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum install java-1.8.0 -y
sudo amazon-linux-extras install epel
sudo yum install jenkins -y
sudo service jenkins start

sudo cat
/var/lib/jenkins/secrets/initialAdminPassword
- Make sure to go to port 8080 to reach the jenkins server
```
- Go ahead and click on install suggested plugin and setup an initial admin user
- And voila!
![image](https://user-images.githubusercontent.com/43883264/173205576-98fa21d0-345d-48fb-b72e-fc007f1a7da6.png)
- Now let's see how i can push from CodePipeline to Jenkins
![image](https://user-images.githubusercontent.com/43883264/173205601-6a087c7f-0b37-4adc-aa29-f8a302ff7dfe.png)
![image](https://user-images.githubusercontent.com/43883264/173205619-5d317429-39c7-424a-b207-3269642024d4.png)

- Not gonna do this just yet, but this is possible too.


## Jenkins AWS Plugins
- Now you remember how we need slaves for Jenkins to run for a more prod based setup like so:
![image](https://user-images.githubusercontent.com/43883264/173207732-44c458aa-c236-4b24-9c0c-d9712e20f83d.png)
- The same way we have Jenkins plugins (EC2 Plugin) that will help you do this in the case where we need to build such a setup
- Some important AWS related plugins include: (If you click the plugin, the documentation for the plugin will pop up)
1. Amazon EC2 - notice the intro section
![image](https://user-images.githubusercontent.com/43883264/173207981-54367d02-ce25-4e61-9849-6611b5f17d58.png)
2. AWS CodeBuild
- The idea behing using CodeBuild is basically you don't need to create slaves as separate EC2 instances and CodeBuild will handle all the slave-work for you -  a serverless way basically
![image](https://user-images.githubusercontent.com/43883264/173208032-504e9d09-791b-4632-a213-27cb35ff1df4.png)

3. Amazon Elastic Container Service
- In this service you can now launch serverless/elastic slaves that run actions to do whatever you need
![image](https://user-images.githubusercontent.com/43883264/173208093-c10267c6-5222-4c19-b728-a9a441e074f1.png)

4. CodePipeline
![image](https://user-images.githubusercontent.com/43883264/173208113-b576f631-1a44-4859-ad1d-8d9275e7705d.png)

5. S3 Artifacts 

- You can send artifacts created on Jenkins to S3





