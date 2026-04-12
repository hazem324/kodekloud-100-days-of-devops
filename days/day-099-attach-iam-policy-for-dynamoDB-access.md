#  Attach IAM Policy for DynamoDB Access Using Terraform

The DevOps team is implementing a secure AWS infrastructure by provisioning a **DynamoDB table** with **fine-grained IAM access control** using Terraform. The objective is to ensure that only trusted AWS services can access the table with **restricted (read-only) permissions**.

---

##  Task Objectives

As part of the Nautilus DevOps Team, the following were implemented:

1. Create a **DynamoDB table** named `nautilus-table`
2. Create an **IAM Role** named `nautilus-role`
3. Create an **IAM Policy** named `nautilus-readonly-policy`
4. Grant **read-only access** (`GetItem`, `Scan`, `Query`) to the table
5. Attach the policy to the IAM role
6. Use Terraform best practices with:

   * `main.tf`
   * `variables.tf`
   * `outputs.tf`
   * `terraform.tfvars`

---

##  Steps

### 1. Variables Definition (`variables.tf`)

```hcl
variable "KKE_TABLE_NAME" {}
variable "KKE_ROLE_NAME" {}
variable "KKE_POLICY_NAME" {}
```

### 2. Variable Values (`terraform.tfvars`)

```hcl
KKE_TABLE_NAME  = "nautilus-table"
KKE_ROLE_NAME   = "nautilus-role"
KKE_POLICY_NAME = "nautilus-readonly-policy"
```

### 3. Main Infrastructure (`main.tf`)

This file provisions:

* DynamoDB Table
* IAM Role
* IAM Policy
* Policy Attachment

Key highlights:

* DynamoDB uses **PAY_PER_REQUEST** billing (minimal config)
* IAM policy strictly allows:

  * `dynamodb:GetItem`
  * `dynamodb:Scan`
  * `dynamodb:Query`
* Access is restricted to **only the created table ARN**

```hcl


resource "aws_dynamodb_table" "nautilus_table" {
  name         = var.KKE_TABLE_NAME
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "id"

  attribute {
    name = "id"
    type = "S"
  }
}


# IAM Role

resource "aws_iam_role" "nautilus_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
}

# IAM Policy (Read-only)

resource "aws_iam_policy" "nautilus_policy" {
  name = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "dynamodb:GetItem",
          "dynamodb:Scan",
          "dynamodb:Query"
        ]
        Resource = aws_dynamodb_table.nautilus_table.arn
      }
    ]
  })
}

# Attach Policy to Role

resource "aws_iam_role_policy_attachment" "attach_policy" {
  role       = aws_iam_role.nautilus_role.name
  policy_arn = aws_iam_policy.nautilus_policy.arn
}
```

### 4. Outputs (`outputs.tf`)

```hcl
output "kke_dynamodb_table" {
  value = aws_dynamodb_table.nautilus_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.nautilus_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.nautilus_policy.name
}
```

##  Deployment Steps

```bash
cd /home/bob/terraform

terraform init
terraform validate
terraform plan
terraform apply -auto-approve
```

---

##  Good To Know

###  IAM & Security Best Practices

* **Principle of Least Privilege**

  * Only allow required actions (`GetItem`, `Scan`, `Query`)
* **Avoid Wildcards**

  * Never use `"Resource": "*"` in production
* **Service Trust**

  * IAM Role is configured to be assumed by trusted AWS services (e.g., EC2)


###  DynamoDB Insights

* **Minimal Configuration Required**

  * `name`, `hash_key`, `attribute`, `billing_mode`
* **Billing Mode**

  * `PAY_PER_REQUEST` avoids capacity planning
* **Primary Key**

  * Must match defined attribute


###  Terraform Best Practices

* Use `terraform.tfvars` for environment-specific values
* Keep all core resources in **one `main.tf`** (as required)
* Use **outputs** for debugging and integration
* Always run:

  ```bash
  terraform validate
  ```

