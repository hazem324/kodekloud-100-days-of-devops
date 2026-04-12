# Create IAM Policy Using Terraform

In modern cloud environments, managing access securely is a foundational requirement. Using **Amazon Web Services (AWS)** Identity and Access Management (IAM), teams can define fine-grained permissions to control who can access what.

In this task, the goal is to provision a **custom IAM policy** using Terraform that grants **read-only access to EC2 resources**, ensuring users can inspect infrastructure without modifying it.

---

##  Task Overview

You are required to:

* Create an IAM policy named **`iampolicy_jim`**
* Configure it in region **us-east-1**
* Grant **read-only access** to:

  * EC2 instances
  * AMIs (Amazon Machine Images)
  * Snapshots
* Use only:

  ```
  /home/bob/terraform/main.tf
  ```

---

## Steps

### 1. Navigate to Working Directory

```bash
cd /home/bob/terraform
```

### 2. Create `main.tf`

Create the Terraform configuration file:

```bash
nano main.tf
```

Add the following content:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_policy" "ec2_read_only_policy" {
  name        = "iampolicy_jim"
  description = "EC2 read-only access policy"

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Allow",
        Action = [
          "ec2:Describe*",
          "ec2:Get*"
        ],
        Resource = "*"
      }
    ]
  })
}
```

### 3. Initialize and Apply Terraform

Run the following commands:

```bash
terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

---

##  Good To Know

* **Terraform** (Infrastructure as Code) allows you to provision AWS resources declaratively and reproducibly.
* The wildcard **`ec2:Describe*`** is powerful — it includes:

  * Instances
  * Volumes
  * Snapshots
  * Security Groups
* AWS provides a managed policy called:

  * `AmazonEC2ReadOnlyAccess`
  * But creating a **custom policy** gives you tighter control.
* IAM policies **do nothing until attached** to:

  * Users 
  * Groups 
  * Roles 
* Always follow the **Principle of Least Privilege**:

  * Only grant what is strictly necessary.


##  Reference

* Terraform IAM Policy Resource:

  * [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy)

