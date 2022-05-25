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