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
