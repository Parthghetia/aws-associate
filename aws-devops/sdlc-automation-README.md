# SDLC Automation
## Continuous Integration
![image](https://user-images.githubusercontent.com/43883264/168151021-5179848d-466f-4a20-967f-90b7d230e798.png)
- A testing/build server could be something like Jenkins CI/CodeBuild

## Continuous Delivery
![image](https://user-images.githubusercontent.com/43883264/168151337-13e3019a-499f-487f-9b9e-2143569f4aa9.png)

### Continuous Delivery vs Continuous Automation
![image](https://user-images.githubusercontent.com/43883264/168151504-01f5b24b-0845-4960-90e7-c062fe588fef.png)

### Technology Stacks to be used by CI/CD
![image](https://user-images.githubusercontent.com/43883264/168151667-0a9f8cc8-b74c-4435-950b-04b80ffe31ed.png)

# AWS CodeCommit (AWS' own Github)
##### NOTE: When SSHing to your code repo. There is one setting that is missing from the official AWS doc
https://stackoverflow.com/questions/69875520/unable-to-negotiate-with-40-74-28-9-port-22-no-matching-host-key-type-found-th

- For your user to be able to clone and play around with the repo. You need to create HTTPS creds or SSH Keys for CodeCommit. Same as Github. Nothing New!

## Securing Repos and Branches in CodeCommit
Limit changes to branches
https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-conditional-branch.html
- Don't forget you can do this to cover all repos
```
 "Resource": "arn:aws:codecommit:*:*:*",
 ```
 
## Creating Notification Rules and Triggers for your Repo
- This is how to create a notification rule
![image](https://user-images.githubusercontent.com/43883264/168181062-f3863b22-8e3c-45c1-8256-cb8a8fd5d50d.png)
![image](https://user-images.githubusercontent.com/43883264/168181150-76e44ac5-118f-4701-864d-e43741f085ec.png)
![image](https://user-images.githubusercontent.com/43883264/168192874-2a7306d2-9706-4073-92e4-8925d0340006.png)
- Under the SNS topic don't forget to create a subscription to forward the events somewhere

-> Let's see how to create a trigger 
![image](https://user-images.githubusercontent.com/43883264/168332617-d2b2aa33-c26e-4073-8745-8bcef08834e9.png)

-> The best thing to try out here, would be to create an event pattern in eventBridge. This would actually help trigger other more advanced functions like pipelines, codeBuild projects etc. More to come.

## CodeCommit and AWS Lambda
https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-lambda.html

## CodeBuild
![image](https://user-images.githubusercontent.com/43883264/168378845-6ee20123-b42e-4f02-813f-3d57e779a70f.png)
![image](https://user-images.githubusercontent.com/43883264/168379083-719de7dc-3d27-446e-9b2e-af650595bb0b.png)

- We are going to build a codebuild project and we are going to test whether the index.html file has the world "Congratulations"

![image](https://user-images.githubusercontent.com/43883264/168380040-7f30389b-8f64-4a0d-b8c1-15119a9a477c.png)
![image](https://user-images.githubusercontent.com/43883264/168380085-49ebecde-a838-4f1c-9462-bae7a306cb2e.png)
![image](https://user-images.githubusercontent.com/43883264/168380121-31e6a5cd-eba7-4777-b203-31414887da61.png)

- We can now try to start the build. And thats it. It succeeded
![image](https://user-images.githubusercontent.com/43883264/168380715-247a55e0-8456-4c3d-90cd-729fa329f4f8.png)
![image](https://user-images.githubusercontent.com/43883264/168381309-eaafd81e-b59b-416b-9549-9d7d17351d1e.png)

-> Let's take a look at the build-spec.yml file that was used to run the codeBuild (self explanatory)
```yaml 
version: 0.2

phases: 
    install:
        runtime-versions:
            nodejs: 10
        commands:
            - echo "installing something"
    pre_build:
        commands: 
            - echo "we are in the pre build phase"
    build:
        commands:
            - echo "we are in the build block"
            - echo "we will run some tests"
            - grep -Fq "Congratulations" index.html
    post_build:
        commands:
            - echo "we are in the post build phase"
 ```
 
-> Here is a more complex build-spec.yml sourced from here. A good read before building a CodeBuild PL: https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html

-> In this file something specific i want to note is artifacts
```yaml
artifacts:
  files:
    - location
    - location
  name: artifact-name
  discard-paths: no | yes
  base-directory: location
  exclude-paths: excluded paths
  enable-symlinks: no | yes
  s3-prefix: prefix
  secondary-artifacts:
    artifactIdentifier:
      files:
        - location
        - location
      name: secondary-artifact-name
      discard-paths: no | yes
      base-directory: location
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: no | yes
      base-directory: location
cache:
  paths:
    - path
    - path
```
What are these? These are basically the final products that are created from our CodeBuild run, As the docker containers are ephemeral, if you want to retrieve some files from the job run. You need to enable artifacts accordingly (You could also send them to s3)

- Also note the cache files, this is to speed up your codeBuild process

Here are some codebuild samples to use for specific use cases https://docs.aws.amazon.com/codebuild/latest/userguide/use-case-based-samples.html

One to specifically look at is the docker build sample, for creating docker apps

## CodeBuild Environment Variables
- How to debug what environment variables have been passed. Same like env in gitlab/jenkins
```yaml
pre_build:
    commands:
      - printenv
      - echo Logging in to Amazon ECR...
```
- For sensitive secrets you could save those variables as parameters or as secrets
![image](https://user-images.githubusercontent.com/43883264/168448440-33a7cd88-9a33-429f-81f6-8fde03930b65.png)
- You can then either check the parameters store in AWS Systems Manager or in Secrets Manager in AWS
- Here is how to do it in the Parameter Store
![image](https://user-images.githubusercontent.com/43883264/168449001-c1a093fe-e73c-48d9-bb8c-f11ed420c95c.png)

![image](https://user-images.githubusercontent.com/43883264/168448661-0bd6dfaf-883f-48d3-96dc-a03af1bbe454.png)

- Be sure that we need a policy attached to the service role of the codebuild:
![image](https://user-images.githubusercontent.com/43883264/168449019-29bf80cb-4c04-424c-9776-e50cc68e47bf.png)

-> Using the AWS Secrets Manager
https://blog.shikisoft.com/define-environment-vars-aws-codebuild-buildspec/

## CodeBuild Artifacts and S3
-> This is basically to store any generated files/stuff out of your pipeline
```yaml
version: 0.2

phases: 
    install:
        runtime-versions:
            nodejs: 10
        commands:
            - echo "installing something"
    pre_build:
        commands: 
            - echo "we are in the pre build phase"
    build:
        commands:
            - echo "we are in the build block"
            - echo "we will run some tests"
            - grep -Fq "Congratulations" index.html
    post_build:
        commands:
            - echo "we are in the post build phase"
            
artifacts:
    files:
      - '**/*'
      - index.html #specifying a particular file
    name: my-webapp-artifacts
 ```
-> Now its time to edit your code build project to reflect where to store the artifacts 
![image](https://user-images.githubusercontent.com/43883264/168708585-daf39998-fef3-4154-a022-add219ef5dce.png)
![image](https://user-images.githubusercontent.com/43883264/168708867-c7998cf1-33f6-4d4f-a934-84acc87d3a13.png)

## Eventbridge, CloudWatch for CodeBuild
-> Setup automatically, can be seen in cloudwatch after a codebuild project is run
-> There is also an automatic dashboard created on Cloudwatch under the metrics tab. To see how long a pipeline has been run and how long it took and all those statistics that can be used to create alarms to avoid overpaying
-> You can also invoke AWS to maybe run a pipeline automatically every hour using Eventbridge by creating a rule like so:
![image](https://user-images.githubusercontent.com/43883264/168712256-f7e7f336-884a-4c86-aff8-c6123c394ab5.png)
![image](https://user-images.githubusercontent.com/43883264/168712275-71cce1df-e07a-44e4-8429-9c40471051c8.png)
![image](https://user-images.githubusercontent.com/43883264/168712326-cd1502ac-a406-47b1-92b1-2738c5bb0f6c.png)

-> Moreover, you can create the rule to maybe run when let's say code is pushed to master or to a certain branch as well but more to come in the CodePipeline Section

### Designing an AWS CodeCommit Pull Request Validation

![image](https://user-images.githubusercontent.com/43883264/168713618-e7183c56-1926-48dd-bb04-88a15d75e95d.png)

### IMPORTANT - Always read DevOps Blogs linked below - Most probably your issue is already solved by someone somewhere
https://aws.amazon.com/blogs/devops/

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
