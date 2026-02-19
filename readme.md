# Static Website Hosting using Amazon S3 and CloudFront

This project demonstrates how to host a static website using Amazon S3 and securely deliver it worldwide using Amazon CloudFront (CDN).

By the end of this project, you will:
- Host a static website on Amazon S3
- Configure Amazon CloudFront as a CDN
- Secure your S3 bucket by allowing access only from CloudFront
- Block direct public access to S3

---

# Architecture Overview

User → CloudFront (CDN) → S3 Bucket (Origin)

CloudFront caches content at edge locations globally to reduce latency and improve performance.

---

#  Prerequisites

- AWS Account
- Basic knowledge of S3
- Basic knowledge of CloudFront
- Static website files (index.html, CSS, JS, images)

---

#  Step 1: Login to AWS Console

1. Open AWS Management Console.
2. Login with your credentials.
3. Select region: **US West (Oregon) – us-west-2**

---

#  Step 2: Create an S3 Bucket

1. Go to Services → Search for **S3**
2. Click **Create bucket**

### General Configuration

- Bucket name: `demo-bucket-12345`  
  (Add a unique number because bucket names must be globally unique)
- Region: **us-west-2**

### Permissions

- Enable ACLs
- Uncheck **Block all public access**
- Check acknowledgment box

Click **Create bucket**

---

#  Step 3: Enable Static Website Hosting

1. Open your bucket
2. Go to **Properties**
3. Scroll to **Static website hosting**
4. Click **Edit**
5. Select **Enable**

Fill the following:

- Index document: `index.html`
- Error document: `error.html`

Click **Save changes**

---

#  Step 4: Add Bucket Policy (Temporary Public Access)

Go to:

Bucket → Permissions → Bucket Policy → Edit

Paste the following policy and replace YOUR_BUCKET_NAME:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AddPerm",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```
Click **Save changes**

---

##  Step 5: Upload Website Files

1. Go to **Objects**
2. Click **Upload**
3. Upload all files inside your website folder  
   > ⚠️ Do NOT upload the parent folder
4. Ensure `index.html` is at the root level
5. Click **Upload**

---

##  Step 6: Test Website from S3

1. Go to **Properties**
2. Scroll to **Static website hosting**
3. Copy the **Bucket website endpoint**
4. Open it in a browser

 Your website should load successfully.

---

##  Step 7: Create CloudFront Distribution

1. Go to **Services → Search for CloudFront**
2. Click **Create distribution**

###  Origin Settings

- **Origin Domain**: Select your S3 static website endpoint  
- **Origin Access**: Choose **Origin access control settings**
- Click **Create control setting**

Fill in:

- **Name**: `demo-s3cf-123`
- **Signing behavior**: Sign requests

Click **Create**

---

###  Distribution Settings

- **Web Application Firewall**: Do not enable
- **Price class**: Use only North America and Europe
- **Default root object**: `index.html`

Click **Create distribution**

---

##  Step 8: Update S3 Bucket Policy for CloudFront

1. Open your **CloudFront distribution**
2. Click **Copy policy**
3. Go to **S3 bucket → Permissions → Bucket Policy**
4. Replace existing policy with the copied policy
5. Click **Save changes**

---

##  Step 9: Block Public Access to S3

1. Go to **S3 → Permissions**
2. Click **Edit** under **Block public access**
3. Enable **Block all public access**
4. Save and confirm

###  Expected Result:

- **S3 Website Endpoint** → Returns `403 Access Denied`
- **CloudFront URL** → Works correctly

---

##  Step 10: Access Website via CloudFront

1. Open your **CloudFront distribution**
2. Copy the **Distribution Domain Name**

Example: d123abcxyz.cloudfront.net
---

3. Open it in your browser

 Your website is now served securely via CDN.

---

#  Final Secure Setup

- S3 bucket is private
- Only CloudFront can access S3
- Website is delivered via CDN
- Content is cached globally

---

#  Benefits of Using CloudFront

- Faster global delivery
- Reduced latency
- Secure HTTPS support
- DDoS protection
- Better scalability

---

#  Summary

In this project, you:

- Created an S3 bucket
- Enabled static website hosting
- Uploaded website files
- Tested S3 endpoint
- Created CloudFront distribution
- Configured Origin Access Control
- Updated bucket policy
- Blocked public S3 access
- Accessed website via CloudFront

---

#  Interview Question

## Q: How do you securely host a static website in AWS?

### Answer:

- Store website files in S3
- Create CloudFront distribution
- Configure Origin Access Control
- Update bucket policy
- Block public access
- Use CloudFront URL for access


