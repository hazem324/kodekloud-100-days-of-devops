# Day 14 - Linux Process Troubleshooting

## Task Description

The xFusionCorp production support team detected an Apache service failure on one of the App Servers in the Stratos DC environment.
The objective is to restore and standardize the Apache service across all App Hosts.

---

## Requirements

### 1. Service Validation

* Identify the App Server where Apache is not running.
* Start and enable Apache on that host.

---

### 2. Port Configuration

* Ensure Apache is running on the following port on **all App Servers**:

```
8084
```

---

### 3. Consistency Check

* Verify that Apache is active on every App Server.
* No application content is required at this stage.

---

## Expected Result

* Apache service is up and running on all App Servers.
* All instances are listening on port 8084.
* No service unavailability is reported.

---

End of Task
