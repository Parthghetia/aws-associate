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

