How to Host a Static Website on AWS S3 Using Terraform: Step-by-Step Guide for DevOps Beginners.
================================================================================================

![Terraform](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*zgpRl0bbPpz618h4.png)

**Introduction**
----------------

As a DevOps beginner, deploying your first static website using **Terraform on AWS S3** is one of the best hands-on projects to understand **Infrastructure as Code (IaC)**, AWS services, and real-world DevOps workflows.

In this guide, you will learn **how to automate S3 bucket creation, configure it for static website hosting, and upload your website files using Terraform**.

What is Terraform?
------------------

**Terraform** is an **open-source Infrastructure as Code (IaC) tool** created by HashiCorp that allows you to **define, provision, and manage cloud infrastructure using code**.

![worflow](https://github.com/user-attachments/assets/7f36dab0-04ad-489a-9800-be0d495a8aed)


> Terraform setup : [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

Prerequisites
-------------

*   An AWS account.
*   Terraform installed on your system( Either on VS code or on EC2 instance).
*   AWS CLI configured (`aws configure`).
*   Basic HTML files (`index.html`, `style.css`).

Why this project?
=================

*   Get hands-on with **Terraform basics.**
*   Understand **S3 static website hosting**.
*   Automate infrastructure, avoiding manual AWS console clicks.

Step 1: Create Your Terraform Project
-------------------------------------

1.  Create <main.tf> where all the details regarding the project is present .

```
resource "aws_s3_bucket" "bucket" {
  bucket = var.bucket_name
}
resource "aws_s3_bucket_ownership_controls" "bucket" {
  bucket = aws_s3_bucket.bucket.id
  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}
resource "aws_s3_bucket_public_access_block" "bucket" {
  bucket = aws_s3_bucket.bucket.id
  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
resource "aws_s3_bucket_acl" "bucket" {
  depends_on = [    aws_s3_bucket_ownership_controls.bucket,
    aws_s3_bucket_public_access_block.bucket
  ]
  bucket = aws_s3_bucket.bucket.id
  acl    = "public-read"
}
resource "aws_s3_bucket_policy" "public_read" {
  bucket = aws_s3_bucket.bucket.id
  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [      {
        Effect = "Allow",
        Principal = "*",
        Action = "s3:GetObject",
        Resource = "${aws_s3_bucket.bucket.arn}/*"
      }
    ]
  })
}
resource "aws_s3_object" "index" {
  bucket       = aws_s3_bucket.bucket.id
  key          = "index.html"
  source       = "index.html"
  content_type = "text/html"
}
resource "aws_s3_object" "style" {
  bucket       = aws_s3_bucket.bucket.id
  key          = "style.css"
  source       = "style.css"
  content_type = "text/css"
}
resource "aws_s3_object" "error" {
  bucket       = aws_s3_bucket.bucket.id
  key          = "error.html"
  source       = "error.html"
  content_type = "text/html"
}
resource "aws_s3_bucket_website_configuration" "website" {
  bucket = aws_s3_bucket.bucket.id
  index_document {
    suffix = "index.html"
  }
  error_document {
    key = "error.html"
  }
  depends_on = [aws_s3_bucket_acl.bucket]
}
```

2. create <Variable.tf> , where all the variables are present .

```
variable "bucket_name" {
  default = "myterraformbucket0019"
}
```

3. create <Provider.tf> , which is used to mention which **cloud Provider** we are using for this project .

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "5.10.0"
    }
  }
}
provider "aws" {
  region = "ap-south-1"
}
```

4. Create <output.tf>

```
output "websiteendpoint" {
    value = "aws_s3_bucket.bucket.website_endpoint"
}
```

Step 2: Add Website Files
-------------------------

1.  Create index.html and style.css for the making of the website .

*   **index.html** : [https://github.com/NikhilRaj-2003/terraform-s3-static-website-hosting/blob/main/design/index.html](https://github.com/NikhilRaj-2003/terraform-s3-static-website-hosting/blob/main/design/index.html)
*   **style.css** : [https://github.com/NikhilRaj-2003/terraform-s3-static-website-hosting/blob/main/design/style.css](https://github.com/NikhilRaj-2003/terraform-s3-static-website-hosting/blob/main/design/style.css)

Step 3: Initialize and Deploy
-----------------------------

1.  `terraform init` : **initializes your Terraform working directory**.

*   It **downloads the required provider plugins** (like AWS, Azure) needed to work with your cloud resources.
*   It **sets up the backend configuration** (where your state file will be stored, like local or S3).
*   It prepares the `.terraform` folder inside your project.

![terraform init](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*LAQBmMzCLqGOZ4Z_FUwC5w.png)

2. `terraform plan`: Used to **preview changes** Terraform will make before applying .Reads your **Terraform configuration files** (`.tf` files).

*   Checks the **current state** of your infrastructure.
*   Generates a **detailed execution plan** showing actions needed to match your desired configuration.
*   Indicates **which resources will be created, updated, or destroyed**.
*   Helps you **review and verify expected changes** safely.
*   Reduces the chances of **accidental modifications** to your infrastructure.

![terraform plan](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Z14xaaf8c1nZapqGSxadqw.png)

3. `terraform apply`: Used to **execute actions** required to reach the desired state of your infrastructure.

*   Reads your **Terraform configuration files** (`.tf` files).
*   Runs **after reviewing the plan** shown by `terraform plan` .
*   Automatically **creates, updates, or deletes resources** on your cloud provider.
*   **Prompts for confirmation** before proceeding (unless using `-auto-approve`).

4. After giving terraform apply command it creates S3-Bucket in AWS Account , without opening AWS .

![S3 — Bucket .](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*zRLTzEU2A7T4__7hy9ljEA.png)

Step 4: Access Your Website
===========================

*   Go to your AWS S3 bucket.
*   Go to **Properties → Static Website Hosting**.
*   Copy the **bucket website endpoint URL**
*   Open in your browser and see your Terraform-hosted static website live.

![Website — Hosted using Terraform .](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*BNxKHWRfDNoqwwZ3-uP6yQ.png)

What You Learned
================

1.Hosting a static website on AWS S3 using Terraform.
2.Automating bucket creation, ACL, and public read policies using IaC.
3.Practical DevOps workflow for portfolio-building.

Conclusion
==========

Through this project, you have learned how to **host a static website on AWS S3 using Terraform**, enabling you to understand and apply the principles of **Infrastructure as Code (IaC)**. You automated the creation of an S3 bucket, configured it for static website hosting, and managed public access using Terraform, eliminating manual steps in the AWS console.
