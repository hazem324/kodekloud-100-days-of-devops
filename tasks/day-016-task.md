# Day 16 - Install and Configure Nginx as an LBR

## Task Description

The Nautilus team is migrating a high-traffic application to a high-availability architecture on the Stratos DC infrastructure.
The remaining task is to configure the Load Balancer (LBR) server.

---

## Requirements

### 1. LBR Server Setup

* Install **Nginx** on the Load Balancer server.

---

### 2. Load Balancing Configuration

* Configure HTTP load balancing in Nginx using **all App Servers** as backends.
* Modify only the main configuration file:

```
/etc/nginx/nginx.conf
```

---

### 3. Application Server Constraints

* Do **not** change the existing Apache ports on App Servers.
* Ensure Apache service is running on all App Servers.

---

### 4. Validation

* Access the application using the **StaticApp** button on the top bar.

---

## Expected Result

* Nginx distributes traffic across all App Servers.
* The application is reachable through the Load Balancer.
* No changes are made to Apache configurations.

---

End of Task
