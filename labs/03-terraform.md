# 🏗️ Lab 03 — Provision AWS with Terraform

🎯 **Goal:** Create a VPC + public subnet + S3 bucket with code, then destroy it all.
🧰 **Prereqs:** Terraform ≥ 1.5, AWS CLI configured for a **sandbox** account, a billing alarm set

## 👣 Steps
```hcl
# main.tf
terraform {
  required_providers {
    aws = { source = "hashicorp/aws", version = "~> 5.0" }
  }
  # Remote state (recommended for teams):
  # backend "s3" {
  #   bucket       = "<your-state-bucket>"
  #   key          = "lab03/terraform.tfstate"
  #   region       = "<REGION>"
  #   use_lockfile = true
  # }
}

provider "aws" { region = var.region }

variable "region"  { default = "ap-south-1" }
variable "project" { default = "zero-to-hero" }

resource "aws_vpc" "main" {
  cidr_block = "10.10.0.0/16"
  tags       = { Name = "${var.project}-vpc" }
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.10.1.0/24"
  map_public_ip_on_launch = true
  tags                    = { Name = "${var.project}-public" }
}

resource "aws_s3_bucket" "artifacts" {
  bucket_prefix = "${var.project}-artifacts-"
  force_destroy = true
}

output "vpc_id" { value = aws_vpc.main.id }
output "bucket" { value = aws_s3_bucket.artifacts.bucket }
```

```bash
terraform init
terraform fmt && terraform validate
terraform plan -out=tfplan
terraform apply tfplan
terraform state list
```

## ✅ Verify
`aws ec2 describe-vpcs --filters Name=tag:Name,Values=zero-to-hero-vpc` returns your VPC.

## 🧹 Cleanup
```bash
terraform destroy -auto-approve
```

## 🧠 Interview angle
> **"Why remote state?"** It lets the whole team share one source of truth, **locking** prevents two applies from running at once, and the state file (which can hold secrets) is stored encrypted instead of on someone's laptop. Add `*.tfstate*` and `.terraform/` to `.gitignore`.
