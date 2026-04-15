# Terraform S3 Secure Bucket Project

## 📌 Project Overview

This project demonstrates how to use Terraform to deploy a secure Amazon S3 bucket. This project is intentionally small as the goal is to demonstrate the basics of Terraform creating an AWS resource.

### What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and manage resources on an infrastructure using configuration files.

### 💼 Business Use Case

At a larger scale, organizations use Terraform to provision infrastructure across multiple cloud regions or providers for disaster recovery, ensuring systems remain available even if one environment fails.

At a smaller scale, if a business needs to deploy resources in a different region, there’s no need to manually rebuild the environment.  By updating a configuration value (such as the region) and running a few commands, the same infrastructure can be recreated in minutes.

This project uses Terraform to deploy a secure Amazon S3 bucket with:

- **Versioning enabled** - object recovery & protection
- **Server-side encryption (AES-256)** - data protection at rest
- **Tags** - for metadata and cost tracking

⚠️ Note: AWS enables encryption by default for new S3 buckets, but explicitly defining it in Terraform is a **best practice** because it makes your configuration declarative and clear.

---

## ⚙️ Prerequisites

**Make sure the following tools are installed:**

- **VS Code** (recommended) - see 👉 https://code.visualstudio.com/docs/setup/setup-overview
- **Terraform** -  see 👉 https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
- **AWS CLI** tool -  see 👉 https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- **AWS Toolkit Extension**: Install the "AWS Toolkit" from the VS Code Extensions.
- **AWS credentials configured** - For AWS setup, see 👉 [Configure AWS Credentials](#configure-aws-credentials)

---

**🏗️ Resources Created**

Terraform provisions:

**1. S3 Bucket**
 - Globally unique name
 - Tagged for tracking

**2. Versioning**
 - Protects against accidental deletion
 - Maintains object history

**3. Server-Side Encryption**
 - AES-256 encryption
 - Ensures secure data storage

## 📁 Terraform Directory Structure

The following Terraform configuration files define and manage the infrastructure:

Create a folder called **terraform-s3-secure-bucket** and create the six files listed below inside the folder. Later you will copy code to the configuration files.

```hcl
terraform-s3-secure-bucket/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── provider.tf
├── terraform.tfvars
└── README.md
```

**Config File Breakdown**

- provider.tf → Configures Terraform and AWS provider
- variables.tf → Defines input variables for region and bucket name
- main.tf → Defines S3 bucket resources
- outputs.tf → Displays outputs after deployment
- terraform.tfvars → Stores variable values (bucket name, region)

**Step-by-Step Build Instructions**

**Step 1** — Install Prerequisites
- Install Terraform
- Install AWS CLI
- Configure AWS credentials

The configuration files have been created in a previous step, just copy the code and make sure to **save** each of the files.

**Step 2** — Copy the code below to the **provider.tf**file.

```hcl

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

```

**Step 3** — Copy the code below to the **variables.tf**file

**Note:** For the region, select the region closest to you. In my case I selected **us-east-1**

```hcl

variable "aws_region" {
  description = "AWS region to deploy resources"
  type        = string
  default     = "us-east-1"
}

variable "bucket_name" {
  description = "Name of the S3 bucket"
  type        = string
}
```

**Step 4** — Copy the code below to the **main.tf** file

```hcl

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
```

**Step 5** — Copy the code below to the **outputs.tf** file

```hcl

output "bucket_name" {
  description = "The name of the S3 bucket"
  value       = aws_s3_bucket.secure_bucket.bucket
  }

output "bucket_arn" {
  description = "The ARN of the S3 bucket"
  value       = aws_s3_bucket.secure_bucket.arn
}
```

**Step 6** — Copy the code below to the **terraform.tfvars** file

```hcl

bucket_name = "acme-terraform-demo-bucket-xyf"

⚠️ Use a globally unique bucket name

```
**Deployment Commands**

### 1. Initialize Terraform
```hcl
terraform init
```
### 2. Validate Configuration
```hcl
terraform validate
```
### 3. Preview Changes
```hcl
terraform plan
```

### 4. Apply Configuration
```hcl
terraform apply

- Type yes when prompted.
```

### 5. Verify the S3 bucket was created
You can verify the bucket by running a command below in VS Code.  or Check in the AWS Management Console on S3 Dashboard
```hcl
aws s3 ls 
```

### 5. Verify S3 bucket properties
In the AWS Management console, check for the following properties of the S3 bucket:

- **Versioning enabled** - Under bucket **Properties** Bucket Versioning is **enabled**
- **Server-side encryption (AES-256)** - AWS enables encryption by default for new S3 buckets. It was defined explicitly in Terraform as a practice exercise.
- **Tags** - Under bucket **Properties** in the **Tags** section you will see bucket tags configured in the **main.tf** file

```
### 6. Cleanup (Destroy the Infrastructure)

 When you are finished with your practice session, you can clean up your resources so you won't get charged for running services.
```
To remove all resources:
```hcl
terraform destroy
```
**Key Takeaways**
- Infrastructure can be defined and managed using code
- Security best practices (encryption, versioning) should be explicit
- Terraform enables repeatable and consistent deployments

---

Here is a link to video samples of Terraform hands-on labs from my cloud course:

▶️ [Terraform Demo](https://github.com/yyoung-50/Portfolio/tree/master/cloud-foundations/Cloud%20Demo%20Videos/Terraform%20demo)

---

## Configure AWS Credentials

- Create an IAM user account. Make sure the user has permissions to create an S3 bucket. 
- Setup an **Access Key ID** and **Secret Access Key** in the AWS Management Console. 

To link your AWS account, open the terminal in VS Code and type **aws configure**

The CLI will prompt you for four pieces of information. If data already exists, it will appear in brackets [...]. 

- To keep the existing data, press Enter; 
- To overwrite it, type the new value.

| **Prompt**              | **What to enter**                          |
|------------------------|--------------------------------------------|
| AWS Access Key ID      | Your 20-character alphanumeric ID          |
| AWS Secret Access Key  | Your 40-character secret key               |
| Default region name    | Region closest to you (e.g., us-east-1)    |
| Default output format  | Leave blank                                |
---


Author

Yvonne Young

Cloud /Support Engineer