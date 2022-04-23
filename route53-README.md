# Route 53
## FQDN BREAKDOWN
![image](https://user-images.githubusercontent.com/43883264/164948033-549220cc-a1ae-4174-9308-cc7e4a779c05.png)
### How DNS works - Should get basic with time
![image](https://user-images.githubusercontent.com/43883264/164948323-9669f777-d01f-471a-959d-806e0886669a.png)

## Amazon Route 53 - Breakdown
- Its a highly available, scalable Authoritative (The customer can update the DNS records) DNS server

### Route 53 - Records and Record Types
![image](https://user-images.githubusercontent.com/43883264/164948416-09e36b1a-144f-45f9-a847-038834870b79.png)
![image](https://user-images.githubusercontent.com/43883264/164948443-c7d257be-b1e0-41ff-9c22-d5dab1031800.png)

### Route 53  - Hosted Zones
![image](https://user-images.githubusercontent.com/43883264/164948486-b9b40402-7e99-4dde-befd-2799fdcb0880.png)
![image](https://user-images.githubusercontent.com/43883264/164948511-445e1176-6e8d-404e-8c7e-d1b81ff8e24d.png)

### Route 53 - Registering a Domain

![image](https://user-images.githubusercontent.com/43883264/164948565-b5f186ca-fa10-4bd6-8b6e-2be926f25230.png)
- Make sure to enable Privacy protection
![image](https://user-images.githubusercontent.com/43883264/164948577-e70ea7e0-6feb-4d2c-b969-0360e04ea2cf.png)
- Once the domain is created you should see a hosted zone for your domain as below:
![image](https://user-images.githubusercontent.com/43883264/164948615-004fe203-0da6-43a5-b318-ab66d62cb0c2.png)
- You should see 2 records in the hosted zone as below:
![image](https://user-images.githubusercontent.com/43883264/164948632-109c9743-556d-429b-a9f4-d41ebd0eada1.png)
- The NS server above is the one that shows which are the servers that will give authoritative answer for the domain specified
