# Terraform S3 Secure Bucket Project

## 📌 Project Overview

This project demonstrates how to use Terraform to deploy a secure Amazon S3 bucket.

### What is Terraform?
Terraform is an Infrastructure as Code (IaC) tool that allows you to define and provision infrastructure using configuration files.

### 💼 Business Use Case

This project simulates a real-world scenario where a company needs to securely store data in AWS.

It provisions an S3 bucket with:
- **Versioning enabled** (object recovery & protection)
- **Server-side encryption (AES-256)** (data protection at rest)
- **Tags** (for metadata and cost tracking)

This project demonstrates:
- Providers
- Variables
- Resources
- Outputs
- State management

> ⚠️ Note: AWS enables encryption by default for new S3 buckets, but explicitly defining it in Terraform is a **best practice** because it makes your configuration declarative and clear.

---

## ⚙️ Prerequisites

Make sure the following tools are installed:

- VS Code (recommended)
- Terraform
- AWS CLI
- AWS credentials configured

### Configure AWS Credentials
```bash
aws configure