#  Create VPC Using Terraform

The Nautilus DevOps team is progressively migrating infrastructure to the AWS cloud using an incremental approach. Instead of executing a large-scale migration in one go, tasks are broken into smaller units to ensure better control, reduced risk, and smoother deployment.

In this task, we focus on provisioning a basic AWS networking component using Infrastructure as Code (IaC) with Terraform.

---

##  Objective

* Create an AWS VPC named **`devops-vpc`**
* Use region **`us-east-1`**
* Define any valid **IPv4 CIDR block**
* Use **Terraform**
* Work inside directory:

  ```bash
  /home/bob/terraform
  ```
* Only create:

  ```bash
  main.tf
  ```

---

## 🧾 Terraform Configuration



> ✅ This defines a VPC resource with a standard private network range.

---

##  Steps

### 1. Navigate to working directory

```bash
cd /home/bob/terraform
```
### 2. Create file main.tf 

Create a file named `main.tf`:

```hcl
resource "aws_vpc" "devops_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "devops-vpc"
  }
}
```

### 3. Initialize Terraform

```bash
terraform init
```

### 4. Review Execution Plan

```bash
terraform plan
```

### 5. Apply Configuration

```bash
terraform apply
```

Type:

```bash
yes
```


##  References

* Terraform AWS VPC Resource:
  [https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc)

* CIDR Calculator:
  [https://mxtoolbox.com/subnetcalculator.aspx](https://mxtoolbox.com/subnetcalculator.aspx)

---

##  Good to Know

###  What is Terraform?

**Terraform** is an open-source **Infrastructure as Code (IaC)** tool developed by HashiCorp that allows you to define, provision, and manage infrastructure using declarative configuration files.

Instead of manually creating resources (like VPCs, servers, databases) through cloud consoles, Terraform lets you describe your infrastructure in code and automatically deploy it.


###  What is Terraform Used For?

Terraform is commonly used to:

*  Provision cloud infrastructure (AWS, Azure, GCP)
*  Automate infrastructure deployment
*  Manage resources like:

  * VPCs
  * EC2 instances
  * Load balancers
  * Databases
*  Maintain consistency across environments (dev, staging, prod)
*  Enable version control for infrastructure


###  When Should You Use Terraform?

Use Terraform when:

*  You want to **automate infrastructure creation**
*  You need **repeatable deployments**
*  You are managing **multiple environments**
*  You want **version-controlled infrastructure (GitOps)**
*  You are working in a **DevOps or Cloud environment**

Avoid manual setup when:

*  Infrastructure is complex
*  You need scalability and consistency


###  Terraform Execution Workflow

```text
terraform init → terraform plan → terraform apply
```

* `init`: Initializes project and downloads providers
* `plan`: Shows what changes will be made
* `apply`: Applies the changes to infrastructure


###  Terraform State

* Stored in `terraform.tfstate`
* Keeps track of infrastructure resources
* Enables Terraform to detect changes and maintain consistency


###  CIDR Block Insight

* `10.0.0.0/16` provides:

  * 65,536 IP addresses
  * Range: `10.0.0.0 → 10.0.255.255`


###  Tags vs Resource Names

* `devops_vpc` → internal Terraform identifier
* `Name = "devops-vpc"` → visible in AWS console


###  Provider Configuration Tip

* Terraform loads **all `.tf` files in a directory**
* Avoid duplicate `provider` blocks unless using `alias`


###  Idempotency

Running:

```bash
terraform apply
```

multiple times → no changes if state matches configuration 


###  Cleanup Resources

```bash
terraform destroy
```