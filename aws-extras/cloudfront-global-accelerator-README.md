# AWS CloudFront
![image](https://user-images.githubusercontent.com/43883264/166125636-b67f2b72-e544-4d90-9aff-200ee395d155.png)
### CloudFront - Origins that can be used
![image](https://user-images.githubusercontent.com/43883264/166125702-d45a1826-15de-49ff-84eb-a05f8ddeb7ed.png)

### CloudFront Architecture at a High Level
![image](https://user-images.githubusercontent.com/43883264/166125728-4f15fb71-8203-4d9e-9581-df91e464ac12.png)

### CloudFront - S3 as an Origin
![image](https://user-images.githubusercontent.com/43883264/166125743-ed827603-5b2f-4fca-8394-82b8ea7c66fb.png)

### CloudFront - ALB or EC2 as an Origin
![image](https://user-images.githubusercontent.com/43883264/166125793-52b50003-16fe-4e1f-b85e-d9c420c9563c.png)

### CloudFront - GeoRestriction
![image](https://user-images.githubusercontent.com/43883264/166125838-3eea2a30-f18c-4886-a6f6-b8bff71dfbc7.png)

### CloudFront vs S3 Cross Region Replication
![image](https://user-images.githubusercontent.com/43883264/166125858-cbfeb146-5629-4638-9395-f02af2a914c7.png)

## CloudFront with S3 - Hands On
- Created a bucket first - Normal settings. And add some static website code into it
- Now am going to go ahead and create a CloudFront Distribution as below:
![image](https://user-images.githubusercontent.com/43883264/166125958-8ba13bcc-6823-430e-99ba-8d28dc39bea1.png)
![image](https://user-images.githubusercontent.com/43883264/166125961-d7a20b21-df57-4692-9394-f6c03e4b9398.png)
![image](https://user-images.githubusercontent.com/43883264/166125967-4b4d23f7-0631-47e4-9396-8625926daed2.png)
- Things to pay attention here:
1. Origin Domain - this is our s3 bucket
2. We selected - Yes use an OAI - This is the identity that will be used to access our S3 bucket
3. We selected - Yes Update bucket policy. Previously the bucket policy is empty. Here is what the policy will look like once this distribution is up and running:
![image](https://user-images.githubusercontent.com/43883264/166126231-1a3292e7-82c9-4975-a935-862c8b1908dd.png)

- It would take some time for the distribution to come up. Once its up. Take the distribution domain name and paste it in your browser:
![image](https://user-images.githubusercontent.com/43883264/166126219-d13f6677-1bfb-4c90-b8fd-54701a5db669.png)

- All your static files would now be served through the cloudfront location - Like so:
![image](https://user-images.githubusercontent.com/43883264/166126254-281c1187-5c2e-4d87-a02b-7953493f27bd.png)

## CloudFront Signed URL/Signed Cookies
![image](https://user-images.githubusercontent.com/43883264/166126542-a0d0353a-5373-414f-b579-fb28d11b06fe.png)
#### CloudFront Signed URL - How does it work
![image](https://user-images.githubusercontent.com/43883264/166126566-460b451b-5427-43bc-825f-82fe2445d768.png)

#### CloudFront Signed URL vs S3 Pre-Signed URL
![image](https://user-images.githubusercontent.com/43883264/166126628-6291959c-288b-420a-8a39-1098e0d515b6.png)

## CloudFront - Advanced Options
- CloudFront pricing varies as per location as well. You can check the pricing on the AWS website
- You can however, reduce the number of edge locations for cost reduction. Remove edge locations, that are expensive
##### CloudFront  - Price Classes
![image](https://user-images.githubusercontent.com/43883264/166126676-0b8871ac-7371-4ab2-9b2b-49a178f7ff14.png)
![image](https://user-images.githubusercontent.com/43883264/166126683-ad2f9df8-3e60-4cf6-80de-3a4355054ffb.png)

### CloudFront - Multiple Origin
![image](https://user-images.githubusercontent.com/43883264/166126716-3cc9c372-0a03-49b2-b7b2-7fbc93f28915.png)

### CloudFront - Origin Groups
![image](https://user-images.githubusercontent.com/43883264/166126755-e727c3f5-6f16-46eb-b332-7618c4da7bbc.png)

### CloudFront - Field Level Encryption
![image](https://user-images.githubusercontent.com/43883264/166127144-286a970a-3e09-4b02-9bb8-b87d5817fcf6.png)

# AWS Global Accelerator
- The Problem that AWS global accelerator solves:
![image](https://user-images.githubusercontent.com/43883264/166131244-f3e34abd-a882-4e51-a595-3a93b91f6562.png)

## NOTE: Unicast vs Anycast IP Addresses
- Unicast - one server has one IP address
- Anycast - all servers hold the same IP address and the client is routed to the nearest one

-> Global accelerator uses the Anycast knowledge to work and this is how it works:
![image](https://user-images.githubusercontent.com/43883264/166131314-deffc8dc-6324-457e-ad66-b085a2072ccc.png)

#### AWS Global Accelerator - Features
![image](https://user-images.githubusercontent.com/43883264/166131355-e9fb5f26-97d8-463d-bda2-be6f50e081cc.png)

## AWS Global Accelerator vs CloudFront
![image](https://user-images.githubusercontent.com/43883264/166131393-2baa7905-8069-4f85-b05c-a31d1378333b.png)

## AWS Global Accelerator Hands On
- Not free!!
- Created a normal HTTP application on an EC2 instance in regions US-EAST-1 and AP-SOUTH-1 -> Security group to allow HTTP from anywhere. We then create the accelerator:
- ![image](https://user-images.githubusercontent.com/43883264/166131505-38818860-e116-4730-a8ef-ffcf53415e9d.png)
- Client affinity - this is sort of stickiness, if you want the same edge location be picked for the client
![image](https://user-images.githubusercontent.com/43883264/166131529-101b0ffb-1fde-4c2a-9b71-6c73295ef2af.png)
- Endpoint groups decide where you want traffic to be routed. The traffic dial is in % - in this case 100% of all traffic goes through there
![image](https://user-images.githubusercontent.com/43883264/166131584-20d6d017-f669-4b27-b365-417bd77ff8ff.png)
- You can configure the health checks in the above snap as below:
![image](https://user-images.githubusercontent.com/43883264/166131595-0c6b111a-b63a-46db-b023-2ffb47e06913.png)

- We then add another endpoint group and same health check as above
![image](https://user-images.githubusercontent.com/43883264/166131607-c325e575-df1e-4e26-a2ad-93dc0bb94abb.png)
- To both the Endpoint groups you can now add endpoints:
![image](https://user-images.githubusercontent.com/43883264/166131624-5472cbad-89a1-42b3-9841-7943e9edc5e2.png)

- Accelerator is ready
![image](https://user-images.githubusercontent.com/43883264/166131652-9c5ed9f0-fe79-4880-b00d-6b01ae59e600.png)


