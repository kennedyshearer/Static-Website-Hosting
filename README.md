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



---

## Link the Domain S3 Buckets with Route 53

---

## Create a SSL certificate with AWS Certificate Manager

---

## Create and configure CloudFront distributions

---
