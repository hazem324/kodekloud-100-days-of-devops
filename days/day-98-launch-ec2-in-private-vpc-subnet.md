# Launch EC2 in Private VPC Subnet Using Terraform

The Nautilus DevOps team required the setup of a private Virtual Private Cloud (VPC) along with a subnet to ensure resources deployed within them remain isolated from external networks and can only communicate within the VPC. An EC2 instance was then provisioned inside the private subnet, accessible only from within the VPC for secure internal communication.

---

## Infrastructure Overview

| Resource | Name | Value |
|---|---|---|
| VPC | `nautilus-priv-vpc` | CIDR: `10.0.0.0/16` |
| Subnet | `nautilus-priv-subnet` | CIDR: `10.0.1.0/24` |
| EC2 Instance | `nautilus-priv-ec2` | Type: `t2.micro` |
| Security Group | `nautilus-priv-sg` | Ingress: VPC CIDR only |

---

## Terraform File Structure

```
/home/bob/terraform/
├── main.tf         # VPC, subnet, security group, EC2 instance
├── variables.tf    # Input variables
└── outputs.tf      # Output values
```

---

## Steps

### 1. Create `variables.tf`

Defines the CIDR blocks used by the VPC and subnet.

```hcl
variable "KKE_VPC_CIDR" {
  description = "CIDR block for the VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  description = "CIDR block for the subnet"
  type        = string
  default     = "10.0.1.0/24"
}
```

### 2. Create `main.tf`

Contains all resource definitions: VPC, subnet, security group, and EC2 instance.

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

# Fetch the latest Amazon Linux 2 AMI dynamically
data "aws_ami" "amazon_linux_2" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}

# VPC
resource "aws_vpc" "nautilus_priv_vpc" {
  cidr_block           = var.KKE_VPC_CIDR
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "nautilus-priv-vpc"
  }
}

# Subnet — auto-assign public IP disabled
resource "aws_subnet" "nautilus_priv_subnet" {
  vpc_id                  = aws_vpc.nautilus_priv_vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false

  tags = {
    Name = "nautilus-priv-subnet"
  }
}

# Security Group — ingress restricted to VPC CIDR only
resource "aws_security_group" "nautilus_priv_sg" {
  name        = "nautilus-priv-sg"
  description = "Security group allowing access only from within the VPC"
  vpc_id      = aws_vpc.nautilus_priv_vpc.id

  ingress {
    description = "Allow all traffic from within VPC"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [var.KKE_VPC_CIDR]
  }

  egress {
    description = "Allow all outbound traffic"
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "nautilus-priv-sg"
  }
}

# EC2 Instance
resource "aws_instance" "nautilus_priv_ec2" {
  ami                    = data.aws_ami.amazon_linux_2.id
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.nautilus_priv_subnet.id
  vpc_security_group_ids = [aws_security_group.nautilus_priv_sg.id]

  tags = {
    Name = "nautilus-priv-ec2"
  }
}
```

Key design decisions:
- `map_public_ip_on_launch = false` ensures instances receive only private IPs.
- Security group ingress is locked to `10.0.0.0/16` (VPC CIDR), blocking all external access.
- The AMI data source dynamically fetches the latest Amazon Linux 2 image, avoiding hardcoded AMI IDs.
- `vpc_security_group_ids` is used (not `security_groups`) as required for VPC instances.

### 3. Create `outputs.tf`

Exposes the names of each created resource.

```hcl
output "KKE_vpc_name" {
  description = "The name of the VPC"
  value       = aws_vpc.nautilus_priv_vpc.tags["Name"]
}

output "KKE_subnet_name" {
  description = "The name of the subnet"
  value       = aws_subnet.nautilus_priv_subnet.tags["Name"]
}

output "KKE_ec2_private" {
  description = "The name of the EC2 instance"
  value       = aws_instance.nautilus_priv_ec2.tags["Name"]
}
```

### 4. Deploy

Run the following Terraform commands from the working directory:

```sh
cd /home/bob/terraform
terraform init
terraform plan
terraform apply
```

Final verification:

```
$ terraform plan
No changes. Your infrastructure matches the configuration.
```

---

## Good To Know

- **Private VPC**: Creates an isolated network environment with no internet gateway attached by default.
- **CIDR Planning**: A `/16` VPC allows 65,536 IPs; a `/24` subnet allows 256 IPs (minus 5 AWS-reserved).
- **`map_public_ip_on_launch = false`**: Ensures instances receive only private IPs — no public exposure.
- **Security Group Scope**: VPC-bound security groups provide more granular control than EC2-Classic.
- **AMI Data Source**: Using a data source ensures the latest image without hardcoding AMI IDs.
- **`vpc_security_group_ids`**: Must be used instead of `security_groups` for VPC instances.
- **Resource Dependencies**: Terraform automatically resolves creation order: VPC → Subnet → Security Group → Instance.
- **AWS Reserved IPs**: The first 4 and last IP in each subnet are reserved by AWS.
- **Private Connectivity**: For internet access, the instance would require a NAT Gateway or VPC Endpoints.

---

## References

- [AWS VPC Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)
- [AWS Subnet Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/subnet)
- [AWS Security Group Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group)
- [AWS AMI Data Source](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)
- [AWS Instance Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)