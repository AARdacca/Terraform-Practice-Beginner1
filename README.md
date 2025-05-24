# Terraform Assignment [Mod 8] – EC2 and S3 Resource Lifecycle

## 📋 Objective

Use Terraform to:
- Provision AWS resources: an EC2 instance and an S3 bucket.
- Destroy the resources after verification.

---

## 📁 Project Structure

```
terraform-aws-assignment/
├── main.tf
├── terraform.tfstate (generated after apply)
├── README.md
```

---

## 💪 Prerequisites

- **RHEL-based Linux system**
- Terraform CLI installed:
  ```bash
  sudo yum install -y yum-utils
  sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
  sudo yum -y install terraform
  ```
- AWS CLI installed and configured:
  ```bash
  sudo yum install -y awscli
  aws configure
  ```

---

## 🔧 Terraform Configuration – `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

# Random suffix for unique S3 bucket name
resource "random_id" "bucket_suffix" {
  byte_length = 4
}

# EC2 Instance
resource "aws_instance" "my_ec2" {
  ami           = "ami-0c02fb55956c7d316"  # Amazon Linux 2 (us-east-1)
  instance_type = "t2.micro"

  tags = {
    Name = "TerraformAssignment-EC2"
  }
}

# S3 Bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket        = "terraform-assignment-bucket-${random_id.bucket_suffix.hex}"
  force_destroy = true

  tags = {
    Name = "TerraformAssignment-S3"
  }
}
```

---

## 🚀 Steps to Run

### 1. Initialize Terraform
```bash
terraform init
```

### 2. Apply Configuration to Create Resources
```bash
terraform apply
```
- Confirm with `yes`
- Resources Created:
  - EC2 Instance (`t2.micro`)
  - S3 Bucket with unique name

### 3. Verify Resources in AWS Console
- EC2 Dashboard → Verify instance is running
- S3 Dashboard → Verify bucket is listed

### 4. Destroy Resources After Validation
```bash
terraform destroy
```
- Confirm with `yes`

---

## 📸 Screenshots to Submit

- ✅ EC2 instance in AWS Console  
- ✅ S3 bucket in AWS Console  
- ✅ Terminal: `terraform apply` execution  
- ✅ Terminal: `terraform destroy` execution  
- ✅ Code snippet of `main.tf`

---

## 🏷️ Tags Added (Bonus)

- EC2 Tag: `Name = "TerraformAssignment-EC2"`
- S3 Bucket Tag: `Name = "TerraformAssignment-S3"`

---

## ✅ Notes

- Region used: **us-east-1**
- AMI ID: **ami-0c02fb55956c7d316** (Amazon Linux 2 in us-east-1)
- Instance type: **t2.micro** (eligible for free tier)
