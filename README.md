# 🚀 Static Website Deployment on AWS S3 using GitHub Actions

This project demonstrates how to **host a static website on Amazon S3** and automatically **deploy using GitHub Actions** on every push to the `main` branch (or any branch you choose).

---

## ✅ Overview

- **Host** the static site using **Amazon S3**.
- Use **GitHub Actions** to **automatically deploy** on every push.

---

## 🔧 Step-by-Step Deployment Guide

### ✅ Step 1: Prepare AWS Environment

#### 1. Create an S3 Bucket
- Go to **AWS S3** → **Create bucket**
- Name it: `your-bucket-name`
- **Disable** “Block all public access”
- **Enable** Static Website Hosting:
  - Set `index.html` as the index document

#### 2. Set Bucket Policy (Allow Public Read)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
