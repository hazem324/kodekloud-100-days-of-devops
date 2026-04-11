# Create Security Group Using Terraform

The Nautilus DevOps team is progressively migrating part of their infrastructure to AWS. Instead of performing a large-scale migration all at once, they are breaking the process into smaller, manageable tasks. This approach ensures better control, minimizes risks, and allows efficient resource utilization throughout the migration lifecycle.

In this task, we use Terraform to create a security group inside the default VPC in AWS.

##  Requirements

* Security group name: `datacenter-sg`
* Description: `Security group for Nautilus App Servers`
* Region: `us-east-1`
* Inbound rules:

  * HTTP → Port `80` → Source `0.0.0.0/0`
  * SSH → Port `22` → Source `0.0.0.0/0`
* Use only `main.tf` (no additional `.tf` files)

---

##  Steps

### 1. Create `main.tf`

Navigate to the Terraform directory:

```bash
cd /home/bob/terraform
```

Create the file:

```bash
nano main.tf
```

Add the following configuration: [`file`](../files/terraform_main_d95.tf).


### 2. Initialize Terraform

```bash
terraform init
```

### 3. Validate Configuration

```bash
terraform validate
```

### 4. Plan and Apply

```bash
terraform plan
terraform apply -auto-approve
```

### 5. Verify Resources

```bash
terraform state list
```

You should see the security group resource listed.

---

##  References

* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group
* https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/vpc

---

##  Good to Know

* **Terraform loads all `.tf` files together**
  Even if you create multiple files, Terraform merges them into a single configuration. That’s why having duplicate provider blocks causes errors.

* **Data sources vs Resources**

  * `data` → used to fetch existing infrastructure (like the default VPC)
  * `resource` → used to create new infrastructure
    This distinction is critical in real-world infrastructure design.

* **Implicit dependencies**
  Terraform automatically understands that the security group depends on the VPC because of:

  ```hcl
  vpc_id = data.aws_vpc.default.id
  ```

  No need to manually define dependencies.

* **Idempotency**
  Running `terraform apply` multiple times will not recreate resources unnecessarily. Terraform ensures the infrastructure matches the desired state.

* **State file importance**
  The `terraform.tfstate` file tracks your infrastructure. Losing or modifying it manually can cause inconsistencies or duplication.

* **Security consideration (SSH exposure)**
  Allowing `0.0.0.0/0` on port 22 is acceptable for labs, but in production:

  * Restrict access to specific IP ranges
  * Use bastion hosts or VPNs

* **Best practice: tagging resources**
  In real environments, always add tags:

  ```hcl
  tags = {
    Name = "datacenter-sg"
    Environment = "dev"
  }
  ```

  This helps with cost tracking and management.

* **Region awareness**
  Resources are region-specific in AWS. If you change region, the default VPC and resources will differ.