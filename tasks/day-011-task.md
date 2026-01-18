# Day 11 - Install and Configure Tomcat Server

## Task Description

The Nautilus development team is preparing to deploy a Java-based application on the Stratos DC infrastructure using Apache Tomcat.

---

## Requirements

### 1. Tomcat Installation

* Install **Tomcat** on **App Server 3**.

---

### 2. Port Configuration

* Configure Tomcat to listen on:

```
8088
```

---

### 3. Application Deployment

* Deploy the following WAR file:

```
/tmp/ROOT.war
```

* Ensure the application is accessible on the root context.

---

### 4. Testing

From the Jump Host, verify using:

```
curl http://stapp03:8088
```

---

## Expected Result

* Tomcat is running on port 8088.
* The application loads successfully at the base URL.

---

End of Task
