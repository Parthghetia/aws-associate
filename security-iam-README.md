- [IAM](#iam)
  - [LAB - IAM - Creating users and groups plus adding policies](#lab---iam---creating-users-and-groups-plus-adding-policies)
    - [IAM POLICY STRUCTURE](#iam-policy-structure)
    - [IAM Policy Hands on](#iam-policy-hands-on)
    - [IAM - Password policy](#iam---password-policy)
      - [MFA setup for root account](#mfa-setup-for-root-account)
  - [IAM Roles for Services](#iam-roles-for-services)
    - [Creating an IAM role hands on](#creating-an-iam-role-hands-on)
    - [Usage of an IAM roles in action](#usage-of-an-iam-roles-in-action)
  - [IAM Built-In Security Tools](#iam-built-in-security-tools)
    - [EXTRA STUFF](#extra-stuff)
  - [IAM Policy Hands On - Using the IAM Policy Simulator](#iam-policy-hands-on---using-the-iam-policy-simulator)


# IAM
- So easy stuff for creating users and groups as needed. Main thing here is policies. Basically creating custom access for users and groups. Let's say providing User Alice who is a DevOps only describe permission on EC2 instances. More to follow
- IAM is a global service and for many other services you'll have a regional approach
- Root account not advised to use - gotta create users with admin account in stead
- Inline policies are specific to let's say just one user
- 

## LAB - IAM - Creating users and groups plus adding policies
So its easy. Basically create an IAM user add it to a group, give him/her Admin privileges. Make sure to set an account alias (something readable apart from numbers) 
![image](https://user-images.githubusercontent.com/43883264/161408024-96e39750-156d-4d66-a94e-242383276d46.png)
![image](https://user-images.githubusercontent.com/43883264/161408034-96904656-1f08-4c9e-a49a-5c9746835d7c.png)
![image](https://user-images.githubusercontent.com/43883264/161408043-c81545e6-4f81-48b7-b7b5-eab224f465e1.png)
Alias changed in the right below:
![image](https://user-images.githubusercontent.com/43883264/161408055-e69a303a-44d3-4107-94a2-16ab168cf2dc.png)


### IAM POLICY STRUCTURE
- Uses basically JSON. Here's a breakdown
![image](https://user-images.githubusercontent.com/43883264/161408917-cd6a01ab-eac9-4e03-b6cb-a80f60604dd0.png)


### IAM Policy Hands on
 - The only fun thing here in this lab is that you can go to policies within IAM and get the JSON for the inbuilt policies and basically use this to create your own custom policies like so:
 ![image](https://user-images.githubusercontent.com/43883264/161889394-c95e6409-b8c0-41e9-a5ec-30a3b67d0c57.png)
- You can also use the visual editor as below to create your own policies:
![image](https://user-images.githubusercontent.com/43883264/161889786-e1578591-48f1-40ff-92d1-e937109a528f.png)

### IAM - Password policy

- Basically set a minimum password policy or require specific characters. 
- Also allow IAM users to change their password
- Prevent password re-use
- Require users to change their password after some time

-> Can be changed from account settings on AWS console

#### MFA setup for root account
- Pretty easy. Click here:
![image](https://user-images.githubusercontent.com/43883264/161890518-1da867d3-2d4c-4c8c-8622-34646350c66f.png)
- And just follow


**NOTE: Do not use your root account to create access-keys



## IAM Roles for Services

- Some AWS services will need to perform actions on your behalf and for this you need to create permissions to AWS services like to EC2 with IAM roles
### Creating an IAM role hands on
1. First you need to create a role from the IAM menu like below:
![image](https://user-images.githubusercontent.com/43883264/162265342-7ad63688-40d7-498d-b694-d3787bd5a06e.png)

2. Then choose the policies/permissions to apply to this role like read-only access
![image](https://user-images.githubusercontent.com/43883264/162265513-9bd2518f-31b3-4a88-a5ba-0fa0f6810a8f.png)
You can go as custom as possible with the permissions
THAT'S IT!

### Usage of an IAM roles in action
- Taking a perfect example - you spin up an EC2 instance - You then want to be able to see some information related to let's say IAM services on your AWS account.
- Although an AMI based Linux instance has the awscli configured into, it is a VERY BAD IDEA to login to this instance using your access key. As anyone who connects to this instance could easily retrieve this and hack the hell out of you
- Hence the way out of this is IAM roles - you basically attach an IAM role to the EC2 instance, to allow it to access a particular service. The above created role allows read-only access to IAM
- If attached to an EC2 instance you have, you will be able to view IAM info like below

![image](https://user-images.githubusercontent.com/43883264/162596403-d19bc490-bfd5-492d-a546-48aa351fd267.png)
![image](https://user-images.githubusercontent.com/43883264/162596409-032671e1-c3a1-4e1c-bf71-41fe0d46675d.png)

That's it. Now when you log back into your EC2 instance you can now view IAM related info like so:
```bash
[ec2-user@ip-172-31-93-36 ~]$ aws iam list-users
{
    "Users": [
        {
            "UserName": "parth",
            "Path": "/",
            "CreateDate": "2022-04-03T02:06:33Z",
            "UserId": "AIDAXOFC6AASKX3GZORUQ", 
            "Arn": "arn:aws:iam::511442812964:user/parth"
        }
    ]
}
```



## IAM Built-In Security Tools
![image](https://user-images.githubusercontent.com/43883264/162265884-1c668685-e046-459f-9cac-079cabd76519.png)

You can easily generate this from the IAM menu - pretty straightforward

To access the access-advisor. You gotta go to users and Access advisor like below:
![image](https://user-images.githubusercontent.com/43883264/162266736-c6965c58-82b3-44de-8937-88de3367933f.png)
- Here you can even check the policies that are used to access the policies and you could actually get rid of non-used services

### EXTRA STUFF
## IAM Policy Hands On - Using the IAM Policy Simulator
- You can also use the AWS Policy Generator Tool we used earlier. Reference to the end of S3 section. You can get to this by just googling it
- You can then use the IAM policy simulator to test your policies like below. Google for IAM policy simulator|
![image](https://user-images.githubusercontent.com/43883264/166001939-c1a07917-1763-49e9-be25-090b5631d95c.png)
![image](https://user-images.githubusercontent.com/43883264/166002006-44d86aaf-37e0-43ab-9feb-61914c8e8199.png)
