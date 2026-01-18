# Day 18 - Configure LAMP server

## Task Description

xFusionCorp is preparing its infrastructure to host a WordPress application across the Stratos Datacenter environment.

---

## Requirements

### 1. Application Server Setup

* Install the following on **all App Hosts**:

  * `httpd`
  * `php` and required dependencies
* Configure Apache to listen on:

```
3002
```

---

### 2. Database Server Setup

* Install and configure **MariaDB** on the **DB Server**.

---

### 3. Database Configuration

* Create a database:

```
kodekloud_db2
```

* Create a database user:

```
kodekloud_top
```

* Set the password to:

```
YchZHRcLkL
```

* Grant the user full privileges on `kodekloud_db2`.

---

### 4. Application Verification

* Access the application using the **LBR App button** from the top bar.
* Confirm the following message appears:

```
App is able to connect to the database using user kodekloud_top
```

---

## Expected Result

* Apache and PHP are correctly configured on all app servers.
* MariaDB is operational with the required database and user.
* The application successfully connects to the database.

---

End of Task