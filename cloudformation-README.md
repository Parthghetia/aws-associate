# CloudFormation
- How to save costs on CloudFormation:
![image](https://user-images.githubusercontent.com/43883264/173211942-797ed74e-8776-4d62-8022-a768a9504296.png)
- Benefits of CloudFormation
![image](https://user-images.githubusercontent.com/43883264/173211961-7c30a1b7-0e87-4e6f-98a7-571f2f2465ed.png)

- How CloudFormation Works
![image](https://user-images.githubusercontent.com/43883264/173211990-3ce36f73-3890-4f96-b248-d5e555663447.png)

- How to deploy CloudFormation Templates
![image](https://user-images.githubusercontent.com/43883264/173212021-8526c0ff-8bb9-420f-9618-c809e9e7a92c.png)

### CloudFormation Building Blocks
![image](https://user-images.githubusercontent.com/43883264/173212048-7c84c687-272b-4153-8510-d99365b15b86.png)

## CloudFormation - Create a Stack - Hands On
![image](https://user-images.githubusercontent.com/43883264/173253926-29c7ee82-c98b-4906-a5ec-63a4ae1586ff.png)
![image](https://user-images.githubusercontent.com/43883264/173254111-d22f7c56-f595-458e-988f-b1c61611a544.png)
- Let's create a stack - The stack will just deploy an EC2 instance
![image](https://user-images.githubusercontent.com/43883264/173256950-88a31cf7-8969-46dc-8d1b-b4833c45fc2a.png)
- Here's the code/template applied to achieve this
```yaml
---
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-009d6802948d06e52
      InstanceType: t2.micro
```
![image](https://user-images.githubusercontent.com/43883264/173257045-7934489c-3c0b-4dcb-ac4d-0e7ab9f4d852.png)
![image](https://user-images.githubusercontent.com/43883264/173257075-0229a218-fbab-4d47-aef0-2a4dc08a2963.png)
- In the review you can hit the estimate cost button to determine, how much it would cost you in the long run
![image](https://user-images.githubusercontent.com/43883264/173257094-5b9fa9ec-23e1-452a-9daf-9bd52506b62e.png)
![image](https://user-images.githubusercontent.com/43883264/173257127-17b34e14-5bc6-445d-b7a3-e69bcd04cead.png)
- Once the stack is created this is what we see
![image](https://user-images.githubusercontent.com/43883264/173257160-0a8b4c7d-cada-45e1-a029-6c4da9c42a08.png)
- And Voila! You'll have your instance
- Resources, is a list of what was created and ready for you
![image](https://user-images.githubusercontent.com/43883264/173257208-328c631c-c85f-4711-a662-7c6f79fe0dc5.png)

## CloudFormation - Update and delete stack
- Now let's update our cloud formation template to create and attach two security groups to our instance. Here is the code:
```yaml
---
Parameters:
  SecurityGroupDescription:
    Description: Security Group Description
    Type: String

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-009d6802948d06e52
      InstanceType: t2.micro
      SecurityGroups:
        - !Ref SSHSecurityGroup
        - !Ref ServerSecurityGroup

  # an elastic IP for our instance
  MyEIP:
    Type: AWS::EC2::EIP
    Properties:
      InstanceId: !Ref MyInstance

  # our EC2 security group
  SSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
      - CidrIp: 0.0.0.0/0
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22

  # our second EC2 security group
  ServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: !Ref SecurityGroupDescription
      SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: 0.0.0.0/0
      - IpProtocol: tcp
        FromPort: 22
        ToPort: 22
        CidrIp: 192.168.1.1/32
```
- Just hit update here
![image](https://user-images.githubusercontent.com/43883264/173257310-10945ed9-afbb-4142-b7d2-086cd1daa3b4.png)
![image](https://user-images.githubusercontent.com/43883264/173257326-965114cd-bab1-439d-ad28-4ed47b6bb467.png)

- Check the template above, you'll notice at the very beginning we asked to set the description of our security group by providing a parameter. So we get asked this during stack update:
- ![image](https://user-images.githubusercontent.com/43883264/173257367-7f469887-7c30-4abe-b4e8-12a4d40d197b.png)
- ![image](https://user-images.githubusercontent.com/43883264/173257390-b2fdd51a-6c94-4fdf-a01b-5389526ca2f2.png)
- A change set preview is generated as below
![image](https://user-images.githubusercontent.com/43883264/173257411-75baa96b-7ad9-4624-a403-7f5a10242133.png)
- **Replacement: true** means that the previous EC2 instance will be deleted and that a new instance will be created
- Here are the update logs 
![image](https://user-images.githubusercontent.com/43883264/173257571-580e41c6-31bc-4a71-9fa6-4bcb92a3ce9b.png)

- And as you can see the required security groups were auto created
![image](https://user-images.githubusercontent.com/43883264/173257596-5eb7c776-a0bc-4a88-9615-2a0d1bb04c77.png)

- How do we get rid of all this, its time to now delete a stack. Yes that will delete all stack resources in the right order too!! Amazing right?

## CloudFormation Parameters
![image](https://user-images.githubusercontent.com/43883264/173262478-858b2243-2ab2-4841-8ae9-d4daf6be3c9d.png)

- When should you use a parameter
![image](https://user-images.githubusercontent.com/43883264/173262589-8bc0d2ab-5453-4e15-b98b-878491cc71d1.png)

- EXTRA: Parameters can be controlled using the following settings:

![image](https://user-images.githubusercontent.com/43883264/173262649-5340d34a-e362-4594-8624-4e525c0a1e8b.png)

- How to reference a parameter
  Make sure to check code above to look for it
  ![image](https://user-images.githubusercontent.com/43883264/173262900-f924079f-8269-4489-aa56-91ddd064ec37.png)
  
#### Pseudo Parameters by AWS
![image](https://user-images.githubusercontent.com/43883264/173263004-a110740d-3189-4325-ba8d-967d93f3613f.png)

## CloudFormation Resources
![image](https://user-images.githubusercontent.com/43883264/173263060-ef57a517-ff34-42ed-b17c-48aee46b89c6.png)
- The best place where the doc https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html would be handy is to check the effect it would have on the resource when you update the resource. Let's see
![image](https://user-images.githubusercontent.com/43883264/173263268-5faff818-364b-44db-8bfd-ba48d48bfdfc.png)

## CloudFormation Mappings
![image](https://user-images.githubusercontent.com/43883264/173263821-a82ef7c7-9a38-4dfd-a394-f7396f24ac38.png)

#### Mappings vs Params
![image](https://user-images.githubusercontent.com/43883264/173263884-53480632-321d-46d6-a6e4-5d5b6dc9fcdf.png)

#### Accessing the Mapping Values
![image](https://user-images.githubusercontent.com/43883264/173263982-da4e704a-6cb4-4260-86e4-47ab2020b458.png)


## CloudFormation Outputs
![image](https://user-images.githubusercontent.com/43883264/173474275-38fd2dca-53fa-47ee-9f68-7a86e74ba46d.png)
- Outputs Example
![image](https://user-images.githubusercontent.com/43883264/173474330-4f3e010f-b71d-48a8-98e4-0deae8051f33.png)
- How to import outputs - Cross Stack Reference
![image](https://user-images.githubusercontent.com/43883264/173474488-3ba924df-990c-4d31-a6ad-ea9561d18886.png)

## CloudFormation Conditions
- How to define a condition
![image](https://user-images.githubusercontent.com/43883264/173474749-7eee6630-62ec-4073-bfb5-36f6677efa60.png)
- Conditions can be applied to resources/outputs etc
![image](https://user-images.githubusercontent.com/43883264/173474861-9df3c246-9431-4f79-ae7e-ee51e5710b8e.png)

## CloudFormation Intrinsic Functions
- Some of the popular intrinsic functions
![image](https://user-images.githubusercontent.com/43883264/173475126-d96d762f-2335-454b-a582-2ff2f6fbe87e.png)

##### Fn::Ref
![image](https://user-images.githubusercontent.com/43883264/173475243-91f80f58-66b6-4747-b192-0c0e3976870f.png)

##### Fn::GetAtt
![image](https://user-images.githubusercontent.com/43883264/173475399-78bf090b-dbc2-45f9-b9cd-e361e80fba41.png)

##### Fn::FindInMap
![image](https://user-images.githubusercontent.com/43883264/173475528-51313929-5971-4109-a895-045b77e79809.png)

##### Fn::ImportValue
![image](https://user-images.githubusercontent.com/43883264/173475587-e960b3ee-d476-4127-8c1f-97bee9b3832c.png)

##### Fn::Join
![image](https://user-images.githubusercontent.com/43883264/173475663-dfb347d0-4fca-4e0b-8002-5ad4dc053998.png)

##### Fn::Sub
![image](https://user-images.githubusercontent.com/43883264/173475748-7dbd1712-47ab-419e-b64e-1416d7504117.png)

##### Condition Functions
And, Equals, If, Not, Or - Defined above

## CloudFormation User Data
![image](https://user-images.githubusercontent.com/43883264/174498669-9cb4ab16-b5c8-4454-90be-aabdc7e97d69.png)

### CloudFormation User Data - Hands On
Here is the template we are going to use for CloudFormation - Notice the user-data bit
```yaml
---
Parameters:
  SSHKey:
    Type: AWS::EC2::KeyPair::KeyName
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instance

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-009d6802948d06e52
      InstanceType: t2.micro
      KeyName: !Ref SSHKey
      SecurityGroups:
        - !Ref SSHSecurityGroup
      # we install our web server with user data
      UserData: 
        Fn::Base64: |
          #!/bin/bash -xe
          yum update -y
          yum install -y httpd
          systemctl start httpd
          systemctl enable httpd
          echo "Hello World from user data" > /var/www/html/index.html

  # our EC2 security group
  SSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: SSH and HTTP
      SecurityGroupIngress:
      - CidrIp: 0.0.0.0/0
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22
      - CidrIp: 0.0.0.0/0
        FromPort: 80
        IpProtocol: tcp
        ToPort: 80
```
- So when you create the stack, you can see the parameter asking for the key pair as below:

![image](https://user-images.githubusercontent.com/43883264/174498853-fba912ef-5c05-4bde-bf8b-c3488909fe85.png)

- So now your HTTP server will be automatically running for you. Now if you ssh to your instance and go to:
```
cat /var/log/cloud-init-output.log
```
- You will see all the logs that were run when the user-data script is run. Cool right?
- An issue here is: Even though the user-data script does not run properly, you will notice that the stack-create-complete will finish just fine. This is not ideal. So here is the solution for that:

## CloudFormation - cfn-init
- This is a more better solution compared to the user-data we had before. Let's see how
![image](https://user-images.githubusercontent.com/43883264/174499054-17f74916-4350-40dd-bf9a-35f1456a944f.png)

### CloudFormation - cfn-init - Hands On
- Here is the template we are going to use for the cfn-init to run:
- Now notice below, we are still using the user-data way of doing stuff, but; notice the sub function and also the fact that we first install cfn bootstrap and later start cfn-init
- Check the metadata way of doing stuff now too. We are basically declaring what we need inside the /var/www/html file and enabling httpd
```yaml
---
Parameters:
  SSHKey:
    Type: AWS::EC2::KeyPair::KeyName
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instance

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-009d6802948d06e52
      InstanceType: t2.micro
      KeyName: !Ref SSHKey
      SecurityGroups:
        - !Ref SSHSecurityGroup
      # we install our web server with user data
      UserData: 
        Fn::Base64:
          !Sub |
            #!/bin/bash -xe
            # Get the latest CloudFormation package
            yum update -y aws-cfn-bootstrap
            # Start cfn-init
            /opt/aws/bin/cfn-init -s ${AWS::StackId} -r MyInstance --region ${AWS::Region} || error_exit 'Failed to run cfn-init'
    Metadata:
      Comment: Install a simple Apache HTTP page
      AWS::CloudFormation::Init:
        config:
          packages:
            yum:
              httpd: []
          files:
            "/var/www/html/index.html":
              content: |
                <h1>Hello World from EC2 instance!</h1>
                <p>This was created using cfn-init</p>
              mode: '000644'
          commands:
            hello:
              command: "echo 'hello world'"
          services:
            sysvinit:
              httpd:
                enabled: 'true'
                ensureRunning: 'true'

  # our EC2 security group
  SSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: SSH and HTTP
      SecurityGroupIngress:
      - CidrIp: 0.0.0.0/0
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22
      - CidrIp: 0.0.0.0/0
        FromPort: 80
        IpProtocol: tcp
        ToPort: 80
 ```
 - Now let's look at what happened:
 ```bash
 [root@ip-172-31-85-27 ec2-user]# cat /var/log/cfn-init.log
2022-06-19 20:32:50,841 [INFO] -----------------------Starting build-----------------------
2022-06-19 20:32:50,842 [INFO] Running configSets: default
2022-06-19 20:32:50,843 [INFO] Running configSet default
2022-06-19 20:32:50,844 [INFO] Running config config
2022-06-19 20:33:06,387 [INFO] Yum installed ['httpd']
2022-06-19 20:33:06,395 [INFO] Command hello succeeded
2022-06-19 20:33:06,539 [INFO] enabled service httpd
2022-06-19 20:33:06,733 [INFO] Started httpd successfully
2022-06-19 20:33:06,734 [INFO] ConfigSets completed
2022-06-19 20:33:06,734 [INFO] -----------------------Build complete-----------------------
```
- A much better organized output of what cfn-init did here. Compared to when passing user-data scripts
- However, even here the stack create complete succeeds before the script is run. So the solution to that is:....

## CloudFormation cfn-signal & wait conditions
![image](https://user-images.githubusercontent.com/43883264/174499466-8a460689-3301-4364-bd6c-815df0cf3b1f.png)

  


