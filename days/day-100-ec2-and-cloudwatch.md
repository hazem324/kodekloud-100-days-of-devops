#   EC2 + CloudWatch Alarm with Terraform

##  Overview

In this task, the Nautilus DevOps team provisions an AWS infrastructure using Terraform to ensure application performance monitoring and alerting.

The setup includes:

* An EC2 instance running Ubuntu
* A CloudWatch alarm to monitor CPU utilization
* An SNS topic for sending notifications

This ensures that when CPU usage exceeds a critical threshold, the system automatically triggers alerts.

---

##  Architecture Components

### 1. EC2 Instance

* Name: `nautilus-ec2`
* AMI: `ami-0c02fb55956c7d316`
* Type: `t2.micro`
* Purpose: Host the application

### 2. SNS Topic

* Name: `nautilus-sns-topic`
* Purpose: Send notifications when alarm is triggered

### 3. CloudWatch Alarm

* Name: `nautilus-alarm`
* Metric: CPUUtilization
* Statistic: Average
* Threshold: ≥ 90%
* Evaluation Period: 1 period (5 minutes)
* Action: Notify SNS topic


##  Steps

###  `main.tf`

This file provisions:

* SNS topic
* EC2 instance
* CloudWatch alarm

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_sns_topic" "sns_topic" {
  name = "nautilus-sns-topic"
}

resource "aws_instance" "nautilus_ec2" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "nautilus-ec2"
  }
}

resource "aws_cloudwatch_metric_alarm" "nautilus_alarm" {
  alarm_name          = "nautilus-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = 1
  period              = 300
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  statistic           = "Average"
  threshold           = 90

  dimensions = {
    InstanceId = aws_instance.nautilus_ec2.id
  }

  alarm_description = "Alarm when CPU exceeds 90%"
  alarm_actions     = [aws_sns_topic.sns_topic.arn]
}
```


###  `outputs.tf`

```hcl
output "KKE_instance_name" {
  value = aws_instance.nautilus_ec2.tags["Name"]
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.nautilus_alarm.alarm_name
}
```


##  Deployment 

Navigate to the Terraform directory:

```bash
cd /home/bob/terraform
```

### Initialize Terraform

```bash
terraform init
```

### Review Execution Plan

```bash
terraform plan
```

### Apply Configuration

```bash
terraform apply -auto-approve
```


##  Key Concepts

###  Resource vs Data Source

* `resource` → creates infrastructure
* `data` → reads existing infrastructure

  In this task, SNS is created using `resource` to avoid lookup issues.


###  CloudWatch Alarm Logic

* Period = 300 seconds → 5 minutes
* Evaluation periods = 1 → immediate alert
* Threshold = 90 → triggers when CPU ≥ 90%


###  Dependency Management

Terraform automatically ensures:

```
SNS → EC2 → CloudWatch Alarm
```

using resource references like:

```hcl
aws_sns_topic.sns_topic.arn
```

##  Common Issues

*  Mixing `data` and `resource` for the same SNS topic
*  Wrong AWS region
*  Incorrect InstanceId in alarm dimensions
*  Forgetting to re-run `terraform plan` after apply


##  Best Practices

* Use consistent naming conventions
* Keep all resources in the same region
* Use outputs for verification
* Validate infrastructure state after deployment

