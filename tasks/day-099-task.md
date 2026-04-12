# Day 99 - Attach IAM Policy for DynamoDB Access

## Task Description

Provision a DynamoDB table and secure access to it using an IAM role with a read-only policy.

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

* Variable values:

```
/home/bob/terraform/terraform.tfvars
```

---

### Variables

Define the following variables:

```
KKE_TABLE_NAME
KKE_ROLE_NAME
KKE_POLICY_NAME
```

---

### DynamoDB Table

* Name:

```
nautilus-table
```

* Minimal configuration (e.g., basic primary key setup).

---

### IAM Role

* Name:

```
nautilus-role
```

* Trust policy allowing AWS services to assume the role.

---

### IAM Policy

* Name:

```
nautilus-readonly-policy
```

* Permissions:

```
GetItem
Scan
Query
```

* Scope:

* Restrict access only to the created DynamoDB table.

* Attach this policy to:

```
nautilus-role
```

---

### Outputs

Define outputs:

```
kke_dynamodb_table
kke_iam_role_name
kke_iam_policy_name
```

---

### Variable Values

* Define actual values in:

```
terraform.tfvars
```

---

### Validation

* Ensure Terraform state is consistent:

```
terraform plan → No changes
```

## Expected Result

* DynamoDB table `nautilus-table` is created.
* IAM role `nautilus-role` is created.
* IAM policy `nautilus-readonly-policy` grants read-only access.
* Policy is attached to the IAM role.
* Access is restricted to the specific DynamoDB table.
* Variables and outputs are correctly configured.
* Terraform configuration is fully applied with no pending changes.

End of Task
