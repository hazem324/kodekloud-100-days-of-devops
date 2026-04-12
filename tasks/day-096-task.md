# Day 96 - Create EC2 Instance Using Terraform

## Task Description

Provision an EC2 instance on AWS using Terraform with a specific AMI, instance type, key pair, and default security group.

## Requirements

* Create Terraform configuration file:

```
/home/bob/terraform/main.tf
```

* Configure AWS provider with region:

```
us-east-1
```

### EC2 Instance

* Name tag:

```
devops-ec2
```

* AMI:

```
ami-0c101f26f147fa7fd
```

* Instance type:

```
t2.micro
```

### Key Pair

* Create a new RSA key:

```
devops-kp
```

### Networking

* Attach the instance to the default VPC.
* Use the default security group.

## Expected Result

* `main.tf` exists in the specified directory.
* EC2 instance is defined with the correct AMI and instance type.
* Key pair `devops-kp` is created and associated with the instance.
* Instance uses the default security group.
* Terraform configuration is valid and ready for `terraform init` and `terraform apply`.

End of Task
