# Day 12 - Linux Network Services

## Task Description

A connectivity issue has been detected on one of the App Servers where Apache is not reachable on the expected port.

---

## Requirements

### 1. Problem Identification

* Diagnose the issue using networking and service tools such as:

  * `telnet`
  * `netstat`
  * Similar system utilities

---

### 2. Service Restoration

* Ensure Apache is:

  * Running correctly.
  * Listening on port:

```
8089
```

* Verify that firewall or security settings are not blocking access.

---

### 3. Access Validation

* Confirm Apache is reachable from the Jump Host.

---

### 4. Testing

From the Jump Host, run:

```
curl http://stapp01:8089
```

---

## Expected Result

* Apache responds successfully on port 8089.
* The service is accessible without weakening security controls.

---

## Notes

* Do not modify the existing `index.html` file.

---

End of Task
