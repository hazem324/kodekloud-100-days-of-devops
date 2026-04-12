# Day 98 - Terraform Private VPC, Subnet, and EC2 Deployment

## Task Description

Provision a private AWS infrastructure using Terraform, including a VPC, subnet, and EC2 instance with restricted access.

## Requirements

### Terraform Files

* Main configuration:

```
/home/bob/terraform/main.tf
```

* Variables file:

```
/home/bob/terraform/variables.tf
```

* Outputs file:

```
/home/bob/terraform/outputs.tf
```

---

### Variables

* Define variables:

```
KKE_VPC_CIDR
KKE_SUBNET_CIDR
```

---

### VPC

* Name:

```
nautilus-priv-vpc
```

* CIDR block:

```
10.0.0.0/16
```

---

### Subnet

* Name:

```
nautilus-priv-subnet
```

* CIDR block:

```
10.0.1.0/24
```

* Disable auto-assign public IP.

---

### EC2 Instance

* Name:

```
nautilus-priv-ec2
```

* Instance type:

```
t2.micro
```

* Launch inside the created subnet.

---

### Security Group

* Allow inbound access only from:

```
10.0.0.0/16
```

* Deny all external access.

---

### Outputs

* Define outputs:

```
KKE_vpc_name
KKE_subnet_name
KKE_ec2_private
```

---

### Validation

* Ensure Terraform state is consistent:

```
terraform plan → No changes
```

## Expected Result

* VPC `nautilus-priv-vpc` is created with correct CIDR.
* Subnet `nautilus-priv-subnet` is created without public IP assignment.
* EC2 instance `nautilus-priv-ec2` is deployed within the subnet.
* Security group restricts access to VPC CIDR only.
* Variables and outputs are correctly defined.
* Terraform configuration is fully applied with no pending changes.

End of Task
