# CodePipeline
![image](https://user-images.githubusercontent.com/43883264/170349261-b77665fa-cb85-44b5-b535-3e9908fe41b0.png)
#### High Level Overview
![image](https://user-images.githubusercontent.com/43883264/170349474-45eb8b5a-2a31-49f7-a6c1-08ecb03a5882.png)

## CodePipeline - Creating a pipeline - Hands On
![image](https://user-images.githubusercontent.com/43883264/170351106-07190156-52ab-439b-860f-f98599b688b5.png)
-> A note here, if you choose an S3 bucket with a default location, you may end up with a lot of automatically created S3 buckets. It's suggested to rather go with custom and create your own S3 bucket.

![image](https://user-images.githubusercontent.com/43883264/170351438-6ad5e2f9-4ec3-4175-93ce-930329877bb8.png)
-> Notice the provider option, has options like ECR - this could be really helpful, when let's say you push a new docker image, it would trigger a pipeline and a codebuild followed by a codedeploy automatically to newer EC2 instances
![image](https://user-images.githubusercontent.com/43883264/170351900-6510e341-93ff-4746-b0ac-6f85d15b74bf.png)

-> Different branches will trigger different pipelines, here we are specifically choosing master but that could always be changed

-> For the build provider stage, we could specify different codebuild projects or even add jenkins as shown. More to come later
![image](https://user-images.githubusercontent.com/43883264/170352023-ba261071-b78d-49c4-b9a1-28e77b3cf052.png)

-> For the deploy provider we have the following options, and we will use AWS CodeDeploy for now
![image](https://user-images.githubusercontent.com/43883264/170352119-f1b5d2ab-ec58-4729-9fd4-ba036fdf31b4.png)
-> NOTE: you can specify your CodeDeploy from another region as well
![image](https://user-images.githubusercontent.com/43883264/170352296-7278e33a-ca4f-436e-83c6-0030f397d491.png)

-> Here is the aftermath of the creation of the pipeline
![image](https://user-images.githubusercontent.com/43883264/170353666-df4c36bc-2b2b-4503-8f48-7323e0585806.png)
-> NOTE: the code used here is the same as the codedeploy-demo code

-> Here is the magic, now if you let's say change something on the index.html file holding our app-data. It would be propagated to the instance, and you will notice the change straight away all automated and pushed to your EC2 instance!!!!!!

-> Just to wrap up here, let's take a look at the CloudWatch event (which is now EventBridge) that was automatically generated
![image](https://user-images.githubusercontent.com/43883264/170354707-f8988eda-bc77-45da-9bf4-96a8ad7888ee.png)

## Adding CodeBuild to CodePipeline 
-> In a CodePipeline we have stages and in every stage we have something called action groups
![image](https://user-images.githubusercontent.com/43883264/170359108-6a26071f-2e35-4725-a718-2a9760f4c641.png)
-> Now let's create an action group
![image](https://user-images.githubusercontent.com/43883264/170359225-32d1bbc5-b1b0-4f61-bd70-1306fb0bc02f.png)
![image](https://user-images.githubusercontent.com/43883264/170359560-36cd8fec-4229-459a-bea1-0f8ebb07e575.png)
-> You can add parallel stages by adding actions in the same stage 
![image](https://user-images.githubusercontent.com/43883264/170359760-e75efd05-7928-4798-8b97-218a72f6ac68.png)

-> The build failed in my case because we did not have enough permissions to access the bucket. All you need to do is change the service role to add all new buckets and you are good to go
![image](https://user-images.githubusercontent.com/43883264/170360900-86e00510-9acc-4641-a60e-439b69376fb3.png)

And that's it!!
![image](https://user-images.githubusercontent.com/43883264/170361050-75041c74-0956-4352-9663-725948072d77.png)


### CodePipeline - Artifacts, S3 and Encryption
-> Let's say we want to store our artifacts in another location once a step is run for maybe future use or something of the sort. We can add a parallel action here to do so. Let's do this:
![image](https://user-images.githubusercontent.com/43883264/170391234-86f03d6b-ab78-4e8a-9380-7a5782a3b340.png)
![image](https://user-images.githubusercontent.com/43883264/170391387-84517bcc-6f8d-496d-b64d-7fb231c574fb.png)

-> Remember, there are different artifacts for different services. What i mean is, Codebuild will have its own artifacts and CodePipeline will have its own artifacts

### CodePipeline - Manual Approval Steps
-> So let's add a new stage to our pipeline to deploy to prod instances
![image](https://user-images.githubusercontent.com/43883264/170392049-14f70c30-328c-4d12-949c-63daf0da9717.png)
-> But we want someone to check whether the deployment went well in dev first and then manually approve for it to be deployed to prod
-> For this you need to create an action group
![image](https://user-images.githubusercontent.com/43883264/170392233-8996af80-65e4-4edb-8507-d097c881d3c4.png)
-> What's really helpful below is the URL. You could provide your prod app url to check if its working or not in there
![image](https://user-images.githubusercontent.com/43883264/170392358-4ed9f099-b627-4345-adc0-85a2719e2afb.png)

-> Notice how the steps are sequential and not parallel here
![image](https://user-images.githubusercontent.com/43883264/170392414-68bae577-4d0a-4060-971b-ad9757c74f62.png)


### NOTE: EventBridge with CodePipeline
-> We can easily setup EventBridge to invoke let's say a lambda function to send something to a slack channel like when a pipeline fails

## CodePipeline - All Integrations - Use Cases
https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html

## CodePipeline - Custom Action Jobs with AWS Lambda
-> How Lambda functions can be used in pipelines
![image](https://user-images.githubusercontent.com/43883264/170546523-474326bf-f3de-4dd0-91da-9255e7094938.png)
https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-invoke-lambda-function.html

