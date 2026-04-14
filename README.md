1. **Project Overview**

What is Terraform

Business use case


This project uses Terraform to deploy a secure Amazon S3 bucket with:

- Versioning enabled (object recovery & protection)

- Server‑side encryption (AES‑256)

- Tags for metadata and cost tracking

It demonstrates Terraform fundamentals including providers, variables, resources, outputs, and state management.

This project is intentionally small as the goal is to demonstrate the basics of Terraform creating an AWS resource.

Resources Created
Terraform provisions the following:

1. S3 Bucket
Globally unique bucket name

Tags for metadata

2. Versioning
Protects against accidental deletion

Maintains object history

3. Server‑Side Encryption
AES‑256 encryption

Ensures data is encrypted at rest

Note: AWS does encrypt new S3 bucket by default, but Terraform still allows you to explicitly configure encryption, and doing so is considered a best practice and makes your configuration declarative and explicit for this practice exercise.

2. **Folder Structure**

Create a folder called **terraform-s3-secure-bucket** and create the file six files listed below**. 
Use this exact folder structure:

terraform-s3-secure-bucket/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── terraform.tfvars
└── README.md

Files Explained
provider.tf
Configures Terraform and the AWS provider.

variables.tf
Defines input variables for region and bucket name.

main.tf
Contains all S3 resources:

Bucket
Versioning
Encryption

outputs.tf
Returns useful information after deployment.

terraform.tfvars
Stores variable values (bucket name, region).


3. **Step-by-Step Build Instructions**

**Step 1 — Install prerequisites**

- Terraform installed

- AWS CLI installed

- AWS credentials configured.

Note: 
Create an AWS account with S3 bucket access permissions. 
Create an access key and secret access key.
In VScode run the command **aws configure** to configure AWS credentials.

Step 2 — Create the provider.tf file and add the content below.

terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

*Step 3* — Create variables.tf file and add the content below.

variable "aws_region" {
  description = "AWS region to deploy resources"
  type        = string
  default     = "us-west-2"
}

variable "bucket_name" {
  description = "Name of the S3 bucket"
  type        = string
}

Step 4 — Create main.tf file and add the content below.

resource "aws_s3_bucket" "secure_bucket" {
  bucket = var.bucket_name

  tags = {
    Project = "Terraform S3 Demo"
    Owner   = "Yvonne"
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.secure_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

resource "aws_s3_bucket_server_side_encryption_configuration" "encryption" {
  bucket = aws_s3_bucket.secure_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

Step 5 — Create outputs.tf

output "bucket_name" {
  description = "The name of the S3 bucket"
  value       = aws_s3_bucket.secure_bucket.bucket
}

output "bucket_arn" {
  description = "The ARN of the S3 bucket"
  value       = aws_s3_bucket.secure_bucket.arn
}

Step 6 — Create terraform.tfvars file and add the content. Use a globally unique bucket name.

bucket_name = "yvonne-terraform-demo-bucket"

Step 7 — Initialize Terraform

Run the `terraform init` command.
This downloads the AWS provider and sets up your working directory.

Step 8 — Validate the configuration

Run the command `terraform validate`

Step 9 — Preview the changes

Run the 'terraform plan command

Step 10 — Deploy the infrastructure

Run the `terraform apply' command, and type "yes" when prompted.

Step 11 — Verify the bucket

You can check the bucket was created by running the command "aws s3 ls" or go to the AWS Management console to the S3 bucket.

Step 12 — Destroy the infrastructure

When you are finished with your practice session, you can clean up your resources.
Run the `terraform destroy` command.
