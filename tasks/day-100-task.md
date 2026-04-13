# Day 100 - Terraform EC2 Instance with CloudWatch Alarm

## Task Description

Provision an EC2 instance and configure a CloudWatch alarm to monitor CPU utilization and trigger notifications via SNS.

## Requirements

### Terraform Files

* Main configuration:

```
/home/bob/terraform/main.tf
```

* Outputs file:

```
/home/bob/terraform/outputs.tf
```

---

### EC2 Instance

* Name:

```
nautilus-ec2
```

* AMI:

```
ami-0c02fb55956c7d316
```

* Instance type:

```
t2.micro
```

---

### CloudWatch Alarm

* Name:

```
nautilus-alarm
```

* Metric:

```
CPUUtilization
```

* Statistic:

```
Average
```

* Threshold:

```
>= 90%
```

* Evaluation period:

```
1 period of 5 minutes
```

* Alarm action:

```
SNS Topic: nautilus-sns-topic
```

---

### Outputs

* Define outputs:

```
KKE_instance_name
KKE_alarm_name
```

---

### Validation

* Ensure Terraform state is consistent:

```
terraform plan → No changes
```

## Expected Result

* EC2 instance `nautilus-ec2` is created successfully.
* CloudWatch alarm `nautilus-alarm` monitors CPU utilization.
* Alarm triggers when CPU usage exceeds 90% for 5 minutes.
* Notifications are sent to `nautilus-sns-topic`.
* Outputs correctly display instance and alarm names.
* Terraform configuration is fully applied with no pending changes.

End of Task
