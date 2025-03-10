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


---

## Link the Domain S3 Buckets with Route 53

---

## Create a SSL certificate with AWS Certificate Manager

---

## Create and configure CloudFront distributions

---
