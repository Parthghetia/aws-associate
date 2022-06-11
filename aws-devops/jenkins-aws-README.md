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



