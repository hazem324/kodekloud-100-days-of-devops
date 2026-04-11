# Day 94 - Create VPC Using Terraform

## Task Description

Create an AWS VPC using Terraform in the specified region with a valid IPv4 CIDR block.

## Requirements

* Create Terraform configuration file:

```
/home/bob/terraform/main.tf
```

* Define a VPC resource with:

```
Name: devops-vpc
```

* Set AWS region:

```
us-east-1
```

* Use any valid IPv4 CIDR block (e.g., `10.0.0.0/16`).
* Ensure proper provider configuration for AWS.

## Expected Result

* `main.tf` exists in the specified directory.
* Terraform configuration defines a VPC named `devops-vpc`.
* VPC is configured in region `us-east-1` with a valid CIDR block.
* Configuration is valid and ready for `terraform init` and `terraform apply`.

End of Task
