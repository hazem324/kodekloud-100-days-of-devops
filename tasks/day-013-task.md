# Day 13 - IPtables Installation And Configuration

## Task Description

The Nautilus security team requires additional protection for the Apache service by restricting access to its exposed port across the Stratos DC infrastructure.

---

## Requirements

### 1. Firewall Installation

* Install **iptables** and all required dependencies on **each App Host**.

---

### 2. Access Control

* Block incoming traffic on port:

```
6400
```

* Allow access **only** from the Load Balancer (LBR) host.

---

### 3. Persistence

* Ensure firewall rules persist after system reboot.

---

## Expected Result

* Port 6400 is inaccessible from all sources except the LBR server.
* Firewall configuration remains active after reboot.

---

End of Task
