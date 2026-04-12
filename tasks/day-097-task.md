# day 97 - Create IAM Policy Using Terraform

## Task Description

Create an IAM policy using Terraform that grants read-only access to Amazon EC2 resources.

## Requirements

* Create Terraform configuration file:

```
/home/bob/terraform/main.tf
```

* Configure AWS provider with region:

```
us-east-1
```

### IAM Policy

* Name:

```
iampolicy_jim
```

* Permissions:

  * Allow read-only access to EC2 resources including:

```
DescribeInstances
DescribeImages
DescribeSnapshots
```

* Policy should allow viewing EC2 resources via AWS Console.

## Expected Result

* `main.tf` exists in the specified directory.
* IAM policy `iampolicy_jim` is defined with correct permissions.
* Policy allows read-only access to EC2 resources.
* Terraform configuration is valid and ready for `terraform init` and `terraform apply`.

## Notes

* IAM policies are global but must still be configured through a provider region.
* Ensure AWS credentials are properly configured before running Terraform.

End of Task
