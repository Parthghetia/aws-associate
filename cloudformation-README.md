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




  


