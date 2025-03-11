# 🌐 Static-Website-Hosting

## 📌 Project Description  
This project focuses on hosting a static HTTPS website using Amazon Web Services (AWS).

---

## 🛡️ Skills Gained
✅ **Import a Domain name** on Amazon Web Services (AWS).  
✅ **Create S3 Buckets** to host a website.  
✅ **Configure Route 53** to link the domain name to the website.  
✅ **Create an SSL certificate** for the website using AWS Certificate Manager (ACM).  
✅ **Create CloudFront distributions** to secure the website in HTTPS.  

---

## 🔧 Tools Used  
| Tool            | Description |
|----------------|------------|
| **Amazon S3** | A cloud storage service that lets users store and retrieve data online. |
| **Amazon Route 53** | A scalable and highly available DNS Web service. |
| **AWS Certificate Manager (ACM)** | A service that lets you easily provision, manage, and deploy public and private SSL/TLS certificates for use with AWS services and internal connected resources. |
| **Amazon CloudFront** | A content delivery network (CDN) that helps websites deliver content to users quickly and securely. |

---

## 📋 Table of Contents

1️⃣ [Topology](#topology)

2️⃣ [Use a domain name](#use-a-domain-name)

3️⃣ [Create and configure S3 Buckets](#create-and-configure-s3-buckets)

4️⃣ [Link the domain to S3 Buckets with Route 53](link-the-domain-to-s3-buckets-with-route-53)

5️⃣ [Create a SSL certificate with AWS Certificate Manager](#create-a-ssl-certificate-with-aws-certificate-manager)

6️⃣ [Create and configure CloudFront distributions](#create-and-configure-cloudfront-distributions)

---

## Topology

---

## Use a Domain name

To host a website, you first need a domain name to access eaily and safely. To get a domain name you have two choices: buy that domain name either AWS or another third-party domain regisrar.
For this project, the domain will be bought from AWS as it is relatively simple, especially for begineers like myself.

##### If you have never handled a domain name
The easiest and most comfortable method is to buy your domain at AWS, it is relatively simple to do and there is no hassle with creating hosted zones as AWS will handle that for you.

##### If you have handled domain names with a third-party registrar
If you have a domain registrar in mind then you can use it, but you will need to create a hosted zone and link your domain to it.

### **1️⃣ Buy a domain from AWS**
First, go to *Route 53*, then select Register domain:

![Route 53 Dashboard](https://i.gyazo.com/30c604755f4d1cefd7b0b5c4c041cbe5.png ':size=550')

Then, choose the domain name of choice and check its availability via the **Exact Match** button. In this example, the chosen domain is *dream-big-work-hard-aws.com*, and it is available for most URL extensions. Choose the desired domain extension by clicking **Select**, and finish the purchase by clicking **Proceed to checkout**:

![Choose domain name](https://i.gyazo.com/f4ffea3c9c2aa0f6c2f456368c052c14.png ':size=550')

Now, to take ownership of a domain, it is necessary to provide personal information (name,address,etc.) whcih is important as it proves that the domain belongs to you, so **provide accurate information**.
In addition, there is a Privacy Protection option to prevent contact information from being visible to WHOIS queries on your domain. I recommend you **ACTIVATE IT**.

![Contact Details](https://i.gyazo.com/1386cacdd6239bccce047667a8808d1f.png ':size=500')

To complete purchase of your domain hit **Complete Order** and check your email for confirmation.
You now have possession of your domain name hosted on AWS (which automatically creates hosted zone)!

---

## Create and configure S3 Buckets

To host your static website, you will need to create an S3 Bucket containing the pages of your website, then make it publicly accessible to the world.

In this example, the domain name will be *dream-big-work-hard-aws.com*.

##### Let get started.
Two S3 Buckets will be created: a **main** Bucket which will be public and contain the pages of the website; and a **redirect** Bucket which will redirect traffic it receives to the main bucket.

The main address of the website will use a non-www name (*dream-big-work-hard-aws.com*):

![www](https://i.gyazo.com/53d9f054e73a197c4fb4fa0fa87a1335.png ':size=250')

The redirect address of the website will use the www name (*www<nolink>.dream-big-work-hard-aws.com*):

![non-www](https://i.gyazo.com/fe6f153573a928ee147a491b6e748f8f.png ':size=250')


### **1️⃣ Main S3 Bucket**
To start, create the main S3 Bucket with the same name as the website hosted:

![Main General Config](https://i.gyazo.com/e17bad62174a6846751129c1b16e67b4.png ':size=700')

Then to make the Bucket publicly available to the Internet:

![Public Access](https://i.gyazo.com/1c8dc5da3ed9465b5e7e4bfa2e650459.png)

The main Bucket is now created and now to install file pages for your website. Go to your newly created Bucket and click on **Upload**:

![Upload Objects](https://i.gyazo.com/dbfbed393808535d81dfd88b324be9cb.png)

Add *index.html* and *error.html* files (create/download simple HTML files). Now the files are objects in the main S3 Bucket:

![Add Files](https://i.gyazo.com/3a970cdd6e977361f7669b4a20ddc3f5.png)

Move to **Permissions** to modify the Bucket Policy:

![Edit Bucket policy](https://i.gyazo.com/9ea4f6dce9411e410fbb5e05fcdbfaa8.png)

Insert this policy and modify it slightly before saving:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket_name/*"
            ]
        }
    ]
}
```

This Bucket policy allows:
- Assign read rights to objects via the Action ''s3:GetObject'' ;
- The Targeted Resources are the objects in your S3 Bucket.

:warning: `Do not forget to replace the *bucket_name* field with the name of the S3 Bucket, and keep the /* at the end of the Resource field, otherwise you won't give access to the Bucket objects!`

This is what the new Bucket Policy looks like after applying it:

![Bucket policy added](https://i.gyazo.com/ac3d2320e33459af2fbc0a332a299190.png)

Finally, edit static web hosting by going to the **Static website hosting section**, at the bottom of the **Properties tab**:

![Static Web Hosting disabled](https://i.gyazo.com/e913c3ef2993dbf7f63a3d09584daba5.png)

Start by enabling and hosting your static website, and indicate the name of your index (*index.html*) and error (*error.html*) documents:

![Edit static web hosting main](https://i.gyazo.com/770586fa7dc06bd80a590e14b01bbc55.png)

Save changes and verify the site is accessible from public address. Go back to the **Static website hosting** section to get the temperory URL of the S3 Bucket:

![Copy URL main Bucket](https://i.gyazo.com/8c615c2bf7ff7b786aae86b109029484.png)

Now see what happens after clickling the link: 

![Success main Bucket](https://i.gyazo.com/d03cec90cf1902d94c8e6bc3246a99f8.png)

Great, there is access to the main page of the website (*index.html*), the website is publicly accessible on the internet.
Next up, time to move on to the redirect S3 Bucket.


### **2️⃣ Redirect S3 Bucket**
Create the redirect S3 Bucket that will redirect all its received traffic to the main S3 Bucket.

![Redrirect General Config](https://i.gyazo.com/c10617d2ceabb5ba6ce47d678127eaff.png)

Then, go to **Static website hosting section**, at the bottom of the **Properties tab**:

![Static Web Hosting disabled](https://i.gyazo.com/e913c3ef2993dbf7f63a3d09584daba5.png)

Enable the static website hosting, but this time by **redirecting the requests for an object**. Specify the target Bucket (in this example *dream-big-work-hard-aws.com*), and leave the **Protocol field** as it is for the moment (*none*):

![Edit static web hosting redirect](https://i.gyazo.com/f44367065fc1c0c461d91fbf653ca76f.png)

Get temperory URL of redirect S3 Bucket redirect S3 Bucket:

![Copy URL redirect Bucket](https://i.gyazo.com/959c39f21164afb10447807329f705a1.png)

See what happens:

![Unsuccessful site](https://i.gyazo.com/628474f752c9bd54f67d02c792dd7db9.png)

Two things to take note of:
- The redirection is done: you are redirected to your website (in this example *dream-big-work-hard-aws.com*);
- But the website does not link to anything... `This site can't be reached`.

To fix this, you need to head back into *Route 53* to configure the AWS created hosted zone to indicate somewhere that your domain name redirects to the main S3 Bucket so that you can access the static website.

---

## Link the domain to S3 Buckets with Route 53

First, go to *Route 53* and look for the DNS records of the hosted zone:

![Hosted Zone](https://i.gyazo.com/e0e4770a77c804059f3bd4646d42e9d4.png)

So far user access to the website through the main and redirect web address is not possible as you will be prompted: `This site can't be reached`.

And the goal is to:

- Forward the traffic received on the main web address to the main S3 Bucket;
- Forward the traffic received on the redirect web address to the redirect S3 Bucket.

In order to do this, create new DNS records in the hosted zone, specifically A records (A for Address), which maps a domain to a physical IP address. In this case, AWS will provide the IP address of the Bucket.

##### The first DNS record
Route traffic from the main web address (*dream-big-work-hard-aws.com*):

![Create record main](https://i.gyazo.com/56b7c78327b6d208fdf40ae1c5fba2d3.png)

1. The main address is a non-www, so leave the *record name* as is.
   
2. Select the *Record type* A - to route the traffic to the main S3 Bucket.
   
3. Turn on *Alias* to specify the S3 Bucket of choice that traffic will be routed.
   
4. Choose the *Alias to S3 website endpoint* in the region where the Bucket is located (*us-east-1*), then select the Bucket that automatically displays in the next selection.

5. Select the *Routing Policy* Simple routing.
   
6. Turn off *Evaluate target health* as it is unnecessary since the website is hosted on S3.

After creating the first record, wait a few minutes and check if the website is accessible via the main web address (*dream-big-work-hard-aws.com*):

![First record main](https://i.gyazo.com/7ac3e513c47fb8a641465b5ca155ba03.png)

The website is now acessible through this URL, the first record is complete.

##### The second DNS record
Route the traffic of your redirect web address (*www<nolink>.cif-project.com*):

![Create record redirect](https://i.gyazo.com/c0893899bb92169191a79737392f39a6.png)

1. The redirect address is a www, so add **www** in the *record name* as is.
   
2. Select the *Record type* A - to route the traffic to the main S3 Bucket.
   
3. Turn on *Alias* to specify the S3 Bucket of choice that traffic will be routed.
   
4. Choose the *Alias to S3 website endpoint* in the region where the Bucket is located (*us-east-1*), then select the Bucket that automatically displays in the next selection.

5. Select the *Routing Policy* Simple routing.
   
6. Turn off *Evaluate target health* as it is unnecessary since the website is hosted on S3.

After creating the second record, wait a few minutes and check if the website is accessible via the redirect web address (*www.dream-big-work-hard-aws.com*).

Still no access? Well this is the problem I ran into. Keep moving forward and we'll see if the problem is resolved later on.

Also notice that from the main web address that it is not secure:

![Not secure](https://i.gyazo.com/7f89ea5e582a16e35469c57fdddf27b5.png)

The domain is brand-new so an SSL certificate hasn't been generated meaning the website uses HTTP instead of HTTPS. 

To fix this:
- Create an SSL certificate for the domain by using AWS Certificate Manager (ACM);
- Use CloudFront to make the site issue HTTPS connections, adding the SSL certificate to the website, and redirecting all HTTP traffic to HTTPS.

Doing this will make the website secure and more immune to certain web attacks.

---

## Create a SSL certificate with AWS Certificate Manager

---

## Create and configure CloudFront distributions

---
