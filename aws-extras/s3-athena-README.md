- [S3](#s3)
  - [S3 Hands On](#s3-hands-on)
  - [S3 Versioning](#s3-versioning)
  - [S3 Versioning Hands on](#s3-versioning-hands-on)
  - [S3 Encryption for objects](#s3-encryption-for-objects)
      - [SSE-S3](#sse-s3)
      - [SS3-KMS](#ss3-kms)
      - [SSE-C](#sse-c)
      - [Client Side Encryption](#client-side-encryption)
  - [S3 Encryption Hands On](#s3-encryption-hands-on)
  - [S3 Security and Policies](#s3-security-and-policies)
      - [S3 Bucket Policies](#s3-bucket-policies)
  - [S3 Security and Bucket Policies Hands On](#s3-security-and-bucket-policies-hands-on)
      - [Blocking Public Access to bucket](#blocking-public-access-to-bucket)
  - [S3 Static Websites](#s3-static-websites)
  - [S3 CORS - Cross Origin Resource Sharing](#s3-cors---cross-origin-resource-sharing)
    - [S3 CORS - Hands On](#s3-cors---hands-on)
  - [S3 MFA Delete](#s3-mfa-delete)
    - [S3 MFA - Hands on](#s3-mfa---hands-on)
  - [S3 - Default Encryption vs Bucket Policies](#s3---default-encryption-vs-bucket-policies)
  - [S3 Access logs](#s3-access-logs)
  - [S3 Access Logs Hands on](#s3-access-logs-hands-on)
  - [S3 Replication (CRR - Cross Region Replication and SRR - Same Region Replication)](#s3-replication-crr---cross-region-replication-and-srr---same-region-replication)
  - [S3 Replication Hands On](#s3-replication-hands-on)
  - [S3 Pre-Signed URLS](#s3-pre-signed-urls)
    - [S3 Pre-Signed URLS - Hands On](#s3-pre-signed-urls---hands-on)
  - [S3 Storage Classes](#s3-storage-classes)
    - [S3 Standard - General Purpose](#s3-standard---general-purpose)
    - [S3 Infrequent Access](#s3-infrequent-access)
    - [S3 Glacier Storage Classes](#s3-glacier-storage-classes)
    - [S3 Intelligent Tiering](#s3-intelligent-tiering)
  - [S3 Storage Classes - Hands On](#s3-storage-classes---hands-on)
  - [S3 Lifecycle Rules](#s3-lifecycle-rules)
      - [Rules for moving between storage classes (Also available on the AWS website)](#rules-for-moving-between-storage-classes-also-available-on-the-aws-website)
    - [Using Lifecycle Rules - Live Scenarios](#using-lifecycle-rules---live-scenarios)
  - [S3 Analytics - Storage Class Analysis](#s3-analytics---storage-class-analysis)
  - [S3 - Improving/Baseline Performance](#s3---improvingbaseline-performance)
      - [S3 - KMS Limitation](#s3---kms-limitation)
  - [S3 Select and Glacier Select](#s3-select-and-glacier-select)
  - [S3 Event Notifications](#s3-event-notifications)
    - [S3 Event Notification - Hands On](#s3-event-notification---hands-on)
  - [S3 Requester Pays](#s3-requester-pays)
  - [Amazon Athena](#amazon-athena)
  - [Amazon Athena - Hands On](#amazon-athena---hands-on)
  - [Glacier Vault Lock](#glacier-vault-lock)
  - [S3 Object Lock](#s3-object-lock)


# S3
- Infinitely scaling storage
- Buckets must have a globally unique name and are defined at region level

![image](https://user-images.githubusercontent.com/43883264/165485631-7fb4c2eb-e767-4d11-abdb-67e74896e5e2.png)
![image](https://user-images.githubusercontent.com/43883264/165485936-faee8e9c-959e-4f30-85d7-08069b086c3a.png)

## S3 Hands On
![image](https://user-images.githubusercontent.com/43883264/165486823-6fc18e15-4c6e-4fab-91f8-2f82fe795d88.png)
![image](https://user-images.githubusercontent.com/43883264/165486843-a8f4b1f9-b66e-4b05-a409-c56889ad030b.png)

## S3 Versioning
- Gotta enable it at bucket level
- This prevents unintentional deleting and easy to roll back too
- ![image](https://user-images.githubusercontent.com/43883264/165488823-523d5fd1-1a23-48e5-9abf-cff9ed1b06a2.png)

## S3 Versioning Hands on
- Enable from here 
![image](https://user-images.githubusercontent.com/43883264/165489001-1c8d9815-9cad-4d7d-aa82-f67962f8653a.png)
- Enabling the show versions button will show you the versions of the files currently
![image](https://user-images.githubusercontent.com/43883264/165489248-8ed64915-f06f-4974-9b47-be4f51de09ae.png)
- B4 and after enabling bucket versioning
![image](https://user-images.githubusercontent.com/43883264/165489411-aa6762fd-fbd3-4c34-bbba-c7460051199e.png)
- Uploading the same file again, now you can see the versions of the file
![image](https://user-images.githubusercontent.com/43883264/165489637-cd01a66c-029f-46c1-a7a0-92854d0d037a.png)
- When a file is deleted, the file is not actually deleted. Just a delete marker is added like below
![image](https://user-images.githubusercontent.com/43883264/165490888-8fd2af57-003e-47db-a766-d16b70d3e50b.png)
- To completely delete, you need to delete both the delete marker and the file like below (You need to type permanently delete after that):
![image](https://user-images.githubusercontent.com/43883264/165491148-89461c2d-a88f-449b-a1ec-f7da730bfb8b.png)

## S3 Encryption for objects
![image](https://user-images.githubusercontent.com/43883264/165500976-5caceac7-a982-4f20-8a8b-b83c6a7a7f4c.png)

#### SSE-S3
![image](https://user-images.githubusercontent.com/43883264/165650468-1fe07e13-f440-4a5d-8f4b-11b4eec57a16.png)

#### SS3-KMS
![image](https://user-images.githubusercontent.com/43883264/165650581-67118255-521e-4c80-b0de-9d8fec744d3b.png)

#### SSE-C
![image](https://user-images.githubusercontent.com/43883264/165650977-2bd9c050-db70-406a-967d-c05a96f7940c.png)

#### Client Side Encryption
- This is where the object is encrypted before even uploading it to S3
![image](https://user-images.githubusercontent.com/43883264/165651117-50d8dab0-b5d0-42e7-97c2-2e9526dc4a8f.png)

NOTE: S3 exposes both an HTTP and HTTPS endpoint, and HTTPS is mandatory for SSE-C

## S3 Encryption Hands On
![image](https://user-images.githubusercontent.com/43883264/165651798-6fbaf048-b437-42ef-828f-445a1977f8dd.png)
![image](https://user-images.githubusercontent.com/43883264/165651924-bb051a87-0093-4f49-a91a-729d30040704.png)
![image](https://user-images.githubusercontent.com/43883264/165652135-e8eb1d32-be36-4019-9881-62a8f13eacff.png)

- You could also optionally enable encryption at the bucket level so that every file is uploaded as encrypted no matter what. This could be also overrid when the file is being uploaded though


## S3 Security and Policies
![image](https://user-images.githubusercontent.com/43883264/165654394-bd454ff8-f728-4b5c-9d3a-a40b96af7bd1.png)

#### S3 Bucket Policies
![image](https://user-images.githubusercontent.com/43883264/165654540-024a5108-7a74-4061-b89c-d8427151cbec.png)

- Bucket Settings for blocking public access to bucket
![image](https://user-images.githubusercontent.com/43883264/165654726-62ec44cf-3c4b-4ad4-9ee3-c98c76d94660.png)

- More S3 Security
![image](https://user-images.githubusercontent.com/43883264/165654875-b149befb-8cd5-4adf-a487-20d14403c315.png)

## S3 Security and Bucket Policies Hands On
![image](https://user-images.githubusercontent.com/43883264/165655011-4551f96c-4132-48b7-b866-9bc25ad397b0.png)
![image](https://user-images.githubusercontent.com/43883264/165655020-1dfa07b6-0c3e-487b-90ec-69db1c9b6a9a.png)

Above we are going for the policy generator:
**WHAT WE ARE GOING TO DO:**
- Create a policy that only allows SSE-S3 Encrypted files only to be uploaded into the bucket
![image](https://user-images.githubusercontent.com/43883264/165655393-112719a2-0108-415c-99c5-7ff7a2c92252.png)
- What we did above:
1. In the ARN copied the bucket ARN from the previous page like so:
![image](https://user-images.githubusercontent.com/43883264/165655449-2ead25d0-dba5-493f-881e-9a409881cc30.png)

2. In the ARN appended /* at the end to cover all the objects in the bucket as the action selected was to putObjects (basically upload objects)
![image](https://user-images.githubusercontent.com/43883264/165655551-5bbfd8ff-0340-483e-b5b2-3d48282998f9.png)
3. Finally, created a conditional saying, if the header does not contain the key as mentioned. Please deny all packets
4. We will then another condition as below:
![image](https://user-images.githubusercontent.com/43883264/165656039-9229931a-3715-4d83-8626-745b88a8ae99.png)
- This condition is to check that the object is both encrypted and with AES256

-> You can then generate the policy and copy paste it to the console

NOTE: This is not rocket science - A right google should do the trick. Like the above was found here: https://aws.amazon.com/blogs/security/how-to-prevent-uploads-of-unencrypted-objects-to-amazon-s3/

#### Blocking Public Access to bucket
![image](https://user-images.githubusercontent.com/43883264/165656697-06380bbe-491e-4ff4-b882-4cae0818a6b6.png)
You could also disable this at account level like so:
![image](https://user-images.githubusercontent.com/43883264/165656733-03ee19da-a5ca-47e3-bced-84997cc0e706.png)
![image](https://user-images.githubusercontent.com/43883264/165656764-8a8ef395-f983-418c-aa22-4af1f4c8141c.png)

ACLs are another way to protect objects in S3: ![image](https://user-images.githubusercontent.com/43883264/165656826-f9ec98a2-be12-49f1-ab53-d0a34e2b12f4.png)

## S3 Static Websites
![image](https://user-images.githubusercontent.com/43883264/165657170-cb191959-6e90-4128-be69-fd40ea5c74f1.png)
- How to enable static website on the bucket as follows:
![image](https://user-images.githubusercontent.com/43883264/165657719-05e1e9b7-fabf-40db-9338-606c0cd779ef.png)
![image](https://user-images.githubusercontent.com/43883264/165657778-fc54ff99-1362-4031-bfe5-8b4276ede97f.png)

- You'll get a forbidden message as below:
![image](https://user-images.githubusercontent.com/43883264/165657983-f957719d-3ac4-4b84-9dc2-13edb780c44b.png)

Thats because you have disabled public access, and you need to define a public policy to allow access as below
![image](https://user-images.githubusercontent.com/43883264/165658057-61272a21-d99a-4649-b88e-da335e7a4279.png)
![image](https://user-images.githubusercontent.com/43883264/165658175-e6e5f279-25e1-4fc2-9814-6296d2092dc1.png)


## S3 CORS - Cross Origin Resource Sharing
- What is CORS?
![image](https://user-images.githubusercontent.com/43883264/165673463-9e040baa-a50e-4003-88f0-be6c9e10a9f7.png)
![image](https://user-images.githubusercontent.com/43883264/165673650-84294216-383d-467e-8c2b-bd5046cea081.png)

**S3 CORS**
- CORS headers need to be enabled on the Cross Origin Bucket and not the original bucket
![image](https://user-images.githubusercontent.com/43883264/165673927-62f9e7e3-fb30-4ad2-9803-d1c1cca602ce.png)

- Remember CORS means different buckets

### S3 CORS - Hands On
- Creating a new bucket in a different region to start with
In that bucket put an extra-page.html and use the code below to add this to the first bucket and fetch that extra page from the secondary bucket (code is self explanatory)
```html
<html>
    <head>
        <title>My First Webpage</title>
    </head>
    <body>
        <h1>I love coffee</h1>
        <p>Hello world!</p>
    </body>

    <img src="coffee.jpg" width=500/>

    <!-- CORS demo -->
    <div id="tofetch"/>
    <script>
        var tofetch = document.getElementById("tofetch");

        fetch('http://demo-cors-bucket-parth.s3-website.us-east-2.amazonaws.com/extra-page.html')
        .then((response) => { 
            return response.text();
        })
        .then((html) => {
            tofetch.innerHTML = html     
        });
    </script>
</html>
```
- When you now refresh your static website, you won't see the extra page loading. But in the developer tools you'll see this:
![image](https://user-images.githubusercontent.com/43883264/165862223-231ba28e-66fb-4d11-9427-2c9d786e4148.png)

- You need to enable the header as in the error message on the second bucket (CORS bucket)
![image](https://user-images.githubusercontent.com/43883264/165862295-d4572696-ad03-40f8-b29e-1da8e6a7d71a.png)
- Add the JSON settings as below
```json
[
    {
        "AllowedHeaders": [
            "Authorization"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "<url of first bucket with http://...without slash at the end>"
        ],
        "ExposeHeaders": [],
        "MaxAgeSeconds": 3000
    }
]
```
![image](https://user-images.githubusercontent.com/43883264/165862639-c6ac3644-8235-4fa1-8cd5-35e3f69fc9e1.png)


## S3 MFA Delete
- Things to know
![image](https://user-images.githubusercontent.com/43883264/166009573-9818517c-225d-4e2a-97a4-44c00feeabd5.png)

### S3 MFA - Hands on
- Pre-req - set up a virtual MFA device first. As before in the security section
```bash
# generate root access keys
aws configure --profile root-mfa-delete-demo

# enable mfa delete
aws s3api put-bucket-versioning --bucket mfa-demo-stephane --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn-of-mfa-device mfa-code" --profile root-mfa-delete-demo

# disable mfa delete
aws s3api put-bucket-versioning --bucket mfa-demo-stephane --versioning-configuration Status=Enabled,MFADelete=Disabled --mfa "arn-of-mfa-device mfa-code" --profile root-mfa-delete-demo
```
## S3 - Default Encryption vs Bucket Policies
- The first way to enforce bucket encryption is to use policies
![image](https://user-images.githubusercontent.com/43883264/166067361-8982d816-522f-4e23-be50-a0ab9bd2a7fa.png)
- Another way is to use the default encryption option in S3:
- ![image](https://user-images.githubusercontent.com/43883264/166067882-71259a34-41d4-4f9b-abe5-0daecbe9e792.png)
- This could be overrid, when uploading a file in case you have another choice

## S3 Access logs
![image](https://user-images.githubusercontent.com/43883264/166068495-d9f6b8e6-e491-46b1-a2f2-7799168d3437.png)
- S3 access logs: Warnings:
![image](https://user-images.githubusercontent.com/43883264/166068610-53403e28-1eea-47b9-9bb5-7c074c8ffcfb.png)

## S3 Access Logs Hands on
- Create a logging bucket
![image](https://user-images.githubusercontent.com/43883264/166068755-54a5a4b5-4a17-456a-a7df-d0a99649d66a.png)
![image](https://user-images.githubusercontent.com/43883264/166068810-426c7063-79ad-4435-8ce9-3962120c3d1d.png)
- The bucket policy for the logging bucket is updated as shown below. To allow the logging service to access your bucket
![image](https://user-images.githubusercontent.com/43883264/166069265-397d0f58-100a-4b2b-9131-c0025669361c.png)

- Now let's see the logs (takes hours):

![image](https://user-images.githubusercontent.com/43883264/166069378-b1fc5d27-42fb-457c-9970-096156d18499.png)
Click open above and then you can see the logs

## S3 Replication (CRR - Cross Region Replication and SRR - Same Region Replication)
![image](https://user-images.githubusercontent.com/43883264/166069909-ec1069fe-8e06-44bf-887b-39717ba4a3fb.png)
![image](https://user-images.githubusercontent.com/43883264/166070003-2e856b4d-29d6-49c8-86e7-d0e52eb32086.png)

## S3 Replication Hands On
- Make sure versioning is enabled in both buckets. Settings on source bucket as below
![image](https://user-images.githubusercontent.com/43883264/166070778-3d12ab5d-277f-4547-bc68-41f447b9d95d.png)
![image](https://user-images.githubusercontent.com/43883264/166070839-642c2d92-f4ef-47e8-a34b-e97c753428e6.png)
- If you enable this setting in the replication rules. Then the delete markers option needs to be enabled
![image](https://user-images.githubusercontent.com/43883264/166071417-1a8a0ca5-b7b7-4f45-8b3a-adadff074583.png)
**- However, you cannot replicate permanent deletions **

## S3 Pre-Signed URLS
![image](https://user-images.githubusercontent.com/43883264/166086731-fb4dcf9a-e2b9-4d8a-b2b0-a60ee3ac6a40.png)
### S3 Pre-Signed URLS - Hands On
![image](https://user-images.githubusercontent.com/43883264/166086799-5cad0786-8447-4f0e-a61a-a35ce8b91f50.png)
![image](https://user-images.githubusercontent.com/43883264/166086830-9b28b514-d4d6-40b8-b8f0-a6e98fb4bc7e.png)

## S3 Storage Classes
![image](https://user-images.githubusercontent.com/43883264/166086861-d0cbe905-c250-4b2a-9197-0e7ad117a384.png)

### S3 Standard - General Purpose
![image](https://user-images.githubusercontent.com/43883264/166086907-6fdbecc7-9d43-4a88-8e29-40c0ddd60f5d.png)

### S3 Infrequent Access
![image](https://user-images.githubusercontent.com/43883264/166086950-d1356554-95dc-4d47-91e1-a3f22df2a21c.png)

### S3 Glacier Storage Classes
![image](https://user-images.githubusercontent.com/43883264/166086985-c2f7bd56-5791-4e23-a9b0-1eb02ccf6d30.png)

### S3 Intelligent Tiering
![image](https://user-images.githubusercontent.com/43883264/166086999-65d26b64-4ba1-493e-b896-0423beb306e3.png)

You can get the prices of all these classes here: https://aws.amazon.com/s3/pricing/

## S3 Storage Classes - Hands On

![image](https://user-images.githubusercontent.com/43883264/166087057-1b586b2a-0f19-4856-a43a-df1dbfc2fd23.png)

- An object's storage class can be changed even after its uploaded under bucket properties
- You can also automate the movement of files to different storage classes using Lifecycle Rules (Management Tab) as below:

![image](https://user-images.githubusercontent.com/43883264/166087132-2ca0d719-6b1b-46d9-9a25-c0d42c1fed47.png)
![image](https://user-images.githubusercontent.com/43883264/166087189-5eeff945-d987-43d4-b041-b397275f9ce3.png)

## S3 Lifecycle Rules
#### Rules for moving between storage classes (Also available on the AWS website)
![image](https://user-images.githubusercontent.com/43883264/166087466-d19cf2f3-a075-43a7-8706-450981e1459e.png)

![image](https://user-images.githubusercontent.com/43883264/166087532-158f6f59-73af-4df8-bad3-74bbc719a44a.png)

### Using Lifecycle Rules - Live Scenarios
![image](https://user-images.githubusercontent.com/43883264/166087643-f0507006-eeb4-43d6-a398-2b32df7c7581.png)
![image](https://user-images.githubusercontent.com/43883264/166087651-a9bdc856-00ba-475c-8d44-a9366c081215.png)

## S3 Analytics - Storage Class Analysis
![image](https://user-images.githubusercontent.com/43883264/166087759-46047913-6ae2-4e78-9d94-caaefdb02aaf.png)

## S3 - Improving/Baseline Performance
![image](https://user-images.githubusercontent.com/43883264/166087828-b4c6019c-9789-4a76-abd2-20c90d329196.png)
![image](https://user-images.githubusercontent.com/43883264/166087971-b8711043-decb-4322-8a12-16292a995db5.png)
![image](https://user-images.githubusercontent.com/43883264/166088047-db814c3b-a13e-493f-bf54-70654c6cb014.png)

#### S3 - KMS Limitation
![image](https://user-images.githubusercontent.com/43883264/166087916-51f1501f-1226-41f4-accc-9276858e3ef9.png)

## S3 Select and Glacier Select
![image](https://user-images.githubusercontent.com/43883264/166088832-83f5a033-5281-4239-b6d6-a89915813995.png)

## S3 Event Notifications
![image](https://user-images.githubusercontent.com/43883264/166088869-7143c91a-5e7a-4f5a-a4fa-8e9201e8881b.png)
- Using amazon event bridge
![image](https://user-images.githubusercontent.com/43883264/166088921-3d442907-29f0-4847-ab11-4ee0a96a982f.png)

### S3 Event Notification - Hands On
- Go to bucket properties
![image](https://user-images.githubusercontent.com/43883264/166088978-cf217b4c-9fbb-41ae-859a-64eae2e3d76b.png)
![image](https://user-images.githubusercontent.com/43883264/166089034-fbedd0e1-4ad4-4cc7-9e55-bb4462446b5f.png)
![image](https://user-images.githubusercontent.com/43883264/166089048-42280c2b-d821-49b4-8327-38c045d52771.png)

For the events to be sent somewhere, you need to have one of the 3 destinations enabled as below. We will use SQS for this demo:
![image](https://user-images.githubusercontent.com/43883264/166089065-700dd457-3209-4790-9247-0beccd8ee839.png)
![image](https://user-images.githubusercontent.com/43883264/166089089-f834a3b5-6b22-49ad-a848-fd530b8c2162.png)
![image](https://user-images.githubusercontent.com/43883264/166089092-8d68b301-4b02-4998-bc3b-c4aa473f2d4d.png)

For the SQS Queue that is supposed to be created, you must specify a policy that will allow anyone to send a message to the SQS service which was created and attached as below using AWS policy generator:
![image](https://user-images.githubusercontent.com/43883264/166089416-00f57d1e-b61b-4e31-a1d3-c8bac3f52424.png)

You can now see the event notifications being generated now as below: (Make sure to hit poll for messages

![image](https://user-images.githubusercontent.com/43883264/166089500-f64641e7-0ea2-48ab-a0b1-ff832f637352.png)

## S3 Requester Pays
![image](https://user-images.githubusercontent.com/43883264/166089553-abe46a8b-3372-4c7b-8f0b-0ec9513f7994.png)

## Amazon Athena
![image](https://user-images.githubusercontent.com/43883264/166089595-4d96d860-59e1-4839-88c6-5224f50f0fc9.png)

## Amazon Athena - Hands On

For Ref: https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/13528368#content

https://aws.amazon.com/premiumsupport/knowledge-center/analyze-logs-athena/

## Glacier Vault Lock
![image](https://user-images.githubusercontent.com/43883264/166090760-8726e5b5-d0b1-4d39-927c-dc455b68d3fa.png)
## S3 Object Lock
![image](https://user-images.githubusercontent.com/43883264/166090794-b8205973-e059-4771-8d1e-4ae784e2986b.png)

