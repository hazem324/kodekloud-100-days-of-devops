# Day 7 - Linux SSH Authentication

## Task Description

The xFusionCorp system administration team requires automated access from the Jump Host to all App Servers in the Stratos DC environment.

---

## Requirements

### 1. SSH Configuration

* Configure **password-less SSH authentication** from:

```
thor@jump_host
```

to all App Servers using their respective sudo users (e.g., `tony` on App Server 1).

---

### 2. Access Scope

* Ensure the connection does not prompt for a password.
* The configuration must support non-interactive script execution.

---

## Expected Result

* The user `thor` can SSH into all App Servers without password prompts.
* Automation scripts execute successfully across all hosts.

---

End of Task
