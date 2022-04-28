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
- 
