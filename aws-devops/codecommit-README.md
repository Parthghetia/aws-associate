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