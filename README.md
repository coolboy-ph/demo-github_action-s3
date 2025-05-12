# 🚀 Static Website Deployment on AWS S3 using GitHub Actions

This project demonstrates how to **host a static website on Amazon S3** and automatically **deploy using GitHub Actions** on every push to the `main` branch (or any branch you choose).

---

## ✅ Overview

- **Host** the static site using **Amazon S3**.
- Use **GitHub Actions** to **automatically deploy** on every push.

---

## 🔧 Step-by-Step Deployment Guide

### Step 1: Prepare AWS Environment

#### ✅ Create an S3 Bucket
- Go to **AWS S3** → **Create bucket**
- Name it: `your-bucket-name`
- **Disable** “Block all public access”
- **Enable** Static Website Hosting:
  - Set `index.html` as the index document

#### ✅ Set Bucket Policy (Allow Public Read)
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
```
#### ✅ Enable Static Website Hosting
- Enable from Properties → “Static website hosting”.
- Note the Endpoint URL (your site URL).

### Step 2: Create IAM User for GitHub
Create an IAM user with programmatic access:
- Permissions: `AmazonS3FullAccess` (or limited access to the bucket)
- Save `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`

### Step 3: Push Code to GitHub
Structure your repo like:

```bash
📁 your-repo/
 ┣ 📄 index.html
 ┣ 📁 .github/workflows/
    ┗ 📄 deploy.yml
```

### Step 4: GitHub Actions CI/CD Setup
Create .github/workflows/deploy.yml:

```yaml
name: Deploy to S3

on:
  push:
    branches:
      - main  # or any branch you prefer

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v3
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1  # change to your region

      - name: Sync to S3
        run: |
          aws s3 sync . s3://your-bucket-name --delete --exclude ".*"
```

### Step 5: Add GitHub Secrets
In your GitHub repo:
Go to Settings → Secrets and variables → Actions → “New repository secret”:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`

### ✅ Final Notes
- Every time you push to main, GitHub Actions will upload your latest app to the S3 bucket.
- You get automatic deployment with minimal setup and zero server management.
