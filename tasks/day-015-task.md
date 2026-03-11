# Day 15 - Setup SSL for Nginx

## Task Description

The xFusionCorp system administration team must prepare App Server 3 for a new application deployment using Nginx with SSL.

---

## Requirements

### 1. Web Server Installation

* Install and configure **Nginx** on **App Server 3**.

---

### 2. SSL Configuration

* Use the provided self-signed certificate and key:

```
/tmp/nautilus.crt
/tmp/nautilus.key
```

* Move them to an appropriate secure location and configure Nginx to use them.

---

### 3. Application Content

* Create an `index.html` file under the Nginx document root with the following content:

```
Welcome!
```

---

### 4. Testing

From the Jump Host, verify using:

```
curl -Ik https://<app-server-ip>/
```

---

## Expected Result

* Nginx serves content over HTTPS.
* The SSL certificate is correctly applied.
* The response contains the “Welcome!” page.

---

End of Task
