# Day 19 - Install and Configure Web Application

## Task Description

xFusionCorp Industries is preparing infrastructure to host two static websites on the Stratos Datacenter environment.

---

## Requirements

### 1. Apache Installation

* Install the **httpd** package and all required dependencies on **App Server 1**.

---

### 2. Port Configuration

* Configure Apache to listen on:

```
3000
```

---

### 3. Website Deployment

Two website backups are available on the Jump Host:

* `/home/thor/media`
* `/home/thor/apps`

Configure Apache so that:

* The **media** site is accessible at:

```
http://localhost:3000/media/
```

* The **apps** site is accessible at:

```
http://localhost:3000/apps/
```

---

### 4. Testing

From App Server 1, verify using:

```
curl http://localhost:3000/media/
curl http://localhost:3000/apps/
```

---

## Expected Result

* Apache runs on port 3000.
* Both websites are served correctly.
* Curl commands return valid content for both paths.

---

End of Task
