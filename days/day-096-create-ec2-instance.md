# Create EC2 Instance Using Terraform

The Nautilus DevOps team is strategizing the migration of part of their infrastructure to AWS. To reduce risk and complexity, the migration is being executed in smaller, controlled steps.

In this task, we provision an EC2 instance using Terraform with specific configuration requirements.

---

## Task Requirements

* Instance Name: `devops-ec2`
* AMI: `ami-0c101f26f147fa7fd` (Amazon Linux)
* Instance Type: `t2.micro`
* Key Pair: `devops-kp`
* Security Group: Default AWS security group
* Constraint: Use **only one file (`main.tf`)**

---

## Steps

We implemented everything inside a single `main.tf` file located at:

```
/home/bob/terraform/main.tf
```

### Resources Used

* `tls_private_key` → Generate RSA private key
* `aws_key_pair` → Register key pair in AWS
* `aws_instance` → Launch EC2 instance
* `data.aws_vpc` → Fetch default VPC
* `data.aws_security_group` → Fetch default security group

---

##  Terraform Configuration

```hcl

resource "tls_private_key" "devops_key" {
  algorithm = "RSA"
  rsa_bits  = 2048
}

resource "aws_key_pair" "devops_kp" {
  key_name   = "devops-kp"
  public_key = tls_private_key.devops_key.public_key_openssh
}

data "aws_vpc" "default" {
  default = true
}

data "aws_security_group" "default" {
  name   = "default"
  vpc_id = data.aws_vpc.default.id
}

resource "aws_instance" "devops_ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  key_name = aws_key_pair.devops_kp.key_name

  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "devops-ec2"
  }
}
```

---

##  Execution Steps

Navigate to the working directory:

```sh
cd /home/bob/terraform
```

Initialize Terraform:

```sh
terraform init
```

Validate configuration:

```sh
terraform validate
```

Preview changes:

```sh
terraform plan
```

Apply configuration:

```sh
terraform apply -auto-approve
```

---

## References

* https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/key_pair
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance

---

##  Good To Know

* **Single File Constraint**: All Terraform configurations (resources, data sources) must be defined in `main.tf` only for this task.
* **Default Security Group**: Terraform does not automatically attach it unless explicitly referenced using a data source.
* **Key Pair Workflow**: `tls_private_key` generates the key locally, while `aws_key_pair` uploads the public key to AWS.
* **AMI Compatibility**: Always ensure the AMI exists in the selected AWS region.
* **Terraform State File**: Tracks infrastructure state — keep it secure and never expose sensitive data.
* **Cost Awareness**: Even `t2.micro` instances may incur charges depending on usage — destroy resources when done:

```sh
terraform destroy -auto-approve
```

