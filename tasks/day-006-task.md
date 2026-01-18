# Day 6 - Create a Cron Job

## Task Description

The Nautilus system administration team needs to validate scheduled task execution on all App Servers in the Stratos DC environment.

---

## Requirements

### 1. Cron Service Setup

* Install the **cronie** package on **all App Servers**.
* Start and enable the **crond** service.

---

### 2. Scheduled Task

* Configure the following cron job for the **root** user:

```
*/5 * * * * echo hello > /tmp/cron_text
```

---

## Expected Result

* The cron service is active on all App Servers.
* The file `/tmp/cron_text` is updated every 5 minutes with the content:

```
hello
```

---

End of Task
