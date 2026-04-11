# Day 95 - Create Security Group Using Terraform

## Task Description

Create an AWS Security Group in the default VPC using Terraform with HTTP and SSH access rules.

## Requirements

* Create Terraform configuration file:

```
/home/bob/terraform/main.tf
```

* Configure AWS provider with region:

```
us-east-1
```

### Security Group

* Name:

```
datacenter-sg
```

* Description:

```
Security group for Nautilus App Servers
```

* Attach to default VPC.

### Inbound Rules

1. HTTP Rule

```
Port: 80
Protocol: TCP
Source: 0.0.0.0/0
```

2. SSH Rule

```
Port: 22
Protocol: TCP
Source: 0.0.0.0/0
```

* Allow all outbound traffic (default behavior acceptable).

## Expected Result

* `main.tf` exists in the specified directory.
* Security group `datacenter-sg` is defined in the default VPC.
* Inbound rules for HTTP (80) and SSH (22) are configured correctly.
* Terraform configuration is valid and ready for `terraform init` and `terraform apply`.

End of Task
