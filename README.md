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

4️⃣ [Link the domain to S3 Buckets with Route 53](#link-the-domain-to-s3-buckets-with-route-53)

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

To create a SSL certificate, go to AWS Certificate Manager and request a certification:

![Request certificate](https://i.gyazo.com/154d99ff3005a65556acc7d70675a7df.png)

Select *Request a public certificate*, as the certificate will be used on a public website:

![Request public certificate](https://i.gyazo.com/825ea25a5e0a17e77967029a4da5b088.png)

When applying for an SSL certificate, you are asked to fill in *Fully Qualified Domain Names* (known as FQDN). In this case, there are two FQDNs: the non-www domain name (e.g. *dream-big-work-hard-aws.com*) and the www domain name (e.g. *www<nolink>.dream-big-work-hard-aws.com*). Fill them both in this same certificate so that the connections by these two addresses are in HTTPS.

Next, choose a validation method, proving that the domain you are creating a certificate for actually belongs to you: imagine if anyone could create an SSL certificate for your site instead of you… Your website would face huge security issues. I advise you to choose the recommended choice, *DNS validation*.

After that, validate the SSL Certificate creation request:

![FQDN and DNS](https://i.gyazo.com/a2657b42ea83958a76c1bd2d89ccb885.png)

The certificate is created, but not validated hence the status **Pending validation**; let's check the configuration:

![Check certificate](https://i.gyazo.com/da94332e12a35cace72873f1ded43822.png)

In the configurations, both DNS records of type CNAME need to be added in the hosted zone. In AWS, most of this is taken care of, so just select **Create records in Route 53**:

![Create records in Route 53](https://i.gyazo.com/b88e4d9063d4a3060e9f488f2087bf47.png)

Select the two FQDNs and finally **Create records**:

![Creation of records](https://i.gyazo.com/055be8a0f07724b1f50ea5237a0cb78b.png)

Now just wait a few minutes for a **Success* status:

![Records created](https://i.gyazo.com/fa6bd5c9dabee90b8227316eb8bca23b.png)

Done! AWS has inputted the CNAME records on your behalf. Verify by checking the hosted zone:

![Hosted zone DNS records](https://i.gyazo.com/da53847027addd18d48a2c93b09ba09e.png)

Now with the CNAME records added, the SSL certificate can be validated.

The certificate is ready, let's move on and apply it to the website with CloudFront.

---

## Create and configure CloudFront distributions

Using CloudFront gives the ability to deploy websites in HTTPS and provide benefits using Edge locations for faster access to the website for users.


### **1️⃣ Main CloudFront distribution**
Before creating the main CloudFront distribution, go to the main S3 Bucket and copy the URL that you can find under the **Properties** tab > **Static website hosting** section:

![Copy URL main](https://i.gyazo.com/e8a31a06688fe4d545b8af93e65706de.png)

Now create the main CloudFront distribution:

![Create distribution main](https://i.gyazo.com/bac585dd3808e85a7066cde2273f80b6.png)

Under **Origin** > **Origin domain**, paste the copied main Bucket URL:

![Origin domain](https://i.gyazo.com/0170a321553c269444634e2dfcd33f19.jpg)

:warning: `Do NOT select drop-down options, those are incorrectly filled in (contains syntax error, I've tried).`

Disable the **Origin shield** to avoid additional charges, and it's not necessary for this project. 

This is what the **Origin** section should look like:

![Origin shield](https://i.gyazo.com/8dc663b0cefbd9810b7e888d274894c9.png)

Next, the **Default cache behavior** section.
In this section, only change the **Viewer protocol policy** to *Redirect HTTP to HTTPS*. 

This is what the **Default cache behavior** section should look like:

![Default cache behavior](https://i.gyazo.com/8e5d27d16c207bab139cb3c43500dbac.png)

Finally, the **Settings** section.
Though the Price class does not effect the Free Tier once you stay within the 2,000,000 HTTP/HTTPS request limits. It will be best practice, to choose the Price Class that fits your needs.

The choice for **Price class** was *Use only North America and Europe*:

![Price Class](https://i.gyazo.com/966788b99f563f7ab5158de58d8e52ac.png)

Fill in an **Alternate domain name (CNAME)** by inserting your main domain name:

![Alternate domain name main](https://i.gyazo.com/89441851b18e0017d0fa51c6705c9bbd.png)

In the **Custom SSL certificate** field, select the newly created certificate:

![Custom SSL certificate](https://i.gyazo.com/cd3deb078151046c01a9ef52951987ed.png)

Leave the **Legacy clients support** unselected to avoid a $600 bill at the end of the month:

![Legacy clients support](https://i.gyazo.com/1eb9a2772ad41a142577f93037c6fe23.png)

Now confirm and create the main CloudFront distribution and move on to starting the redirect CloudFront distribution.


### **2️⃣ Redirect CloudFront distribution**

Same as before, go to the redirect S3 Bucket and copy the URL that you can find under the **Properties** tab > **Static website hosting** section:

![Copy URL redirect](https://i.gyazo.com/5466df69835cdef38364297f341e7399.png)

Now create your second CloudFront distribution. The configuration of the redirect CloudFront distribution will be almost the same, except for two details:
- The **Origin domain** will be the URL of the redirect S3 Bucket:

![Origin domain redirect](https://i.gyazo.com/f01900a37965545b858b30652aa3408a.png)

- Fill in an **Alternate domain name (CNAME)** by inserting your redirect domain name::

![Alternate domain name redirect](https://i.gyazo.com/f822a1c93f5a049d45f07baa055aa580.png)

Once the configuration of your redirect distribution is complete, you can validate its creation.

There are now two CloudFront distributions. Wait a few minutes to let the distributions be validated and enabled.

In the meantime, go to the redirect S3 Bucket, **Properties tab** > **Static website hosting** section, to make a modification: change the used **Protocol** from HTTP to HTTPS:

![Protocol HTTPS](https://i.gyazo.com/18a99e7363adf67e1cd34a3e57eb2578.png)

Go back to CloudFront, where you copied the web addresses of your CloudFront distributions (**Domain name field**) in the navigator search bar, in order to verify that you can access your site through these web addresses:

![Copy distributions domain name](https://i.gyazo.com/9f3a2c040b836ae6ce06063f0aed8a76.png)

Copy the URL of the main CloudFront distribution:

![HTTPS main distribution](https://i.gyazo.com/9727f55abea5a605d06b8acdb1e409b6.png)

The website is accessible, and in HTTPS this time.

Now copy the URL of the redirect CloudFront distribution:

![HTTPS redirect distribution](https://i.gyazo.com/f9823742a3ce7ca591fe63347424aaad.png)

To finalize, head to Route 53 to make changes to the A records, so that the web addresses route traffic to the freshly created Cloudfront distributions:

![Modify A records](https://i.gyazo.com/da53847027addd18d48a2c93b09ba09e.png)

The record for the main domain name should look like this:

![A record main](https://i.gyazo.com/3c5e290825ea3d64beab73f848e1b364.png)

And the record for the redirect domain name should look like that:

![A record redirect](https://i.gyazo.com/7bfe860fb4a791bcaf342442caf9543b.png)

Time to test the accessiblilty of the website through HTTP URLs to verify that the redirection to HTTPS works.

First check the main domain in HTTP (*http://dream-big-work-hard-aws.com*)

![HTTP to HTTPS main](https://i.gyazo.com/9727f55abea5a605d06b8acdb1e409b6.png)

Second check the redirect domain in HTTP (*http://www<nolink>.dream-big-work-hard-aws.com*)

![HTTP to HTTPS redirect](https://i.gyazo.com/f9823742a3ce7ca591fe63347424aaad.png)

Wooray!!!! My first AWS project setting up a **Static Website** in HTTPS on AWS is complete!

---
