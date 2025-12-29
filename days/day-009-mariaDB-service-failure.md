# 🛠️ Debugging MariaDB Service Failure

## 📍 Context

A **critical production issue** was detected in the **Nautilus application** running in **Stratos Datacenter**.  
The application failed to connect to its database.

After investigation, it was confirmed that the **MariaDB service was down** on the database server.

---

## 🚨 Problem Summary

- Application cannot connect to the database
- `mariadb.service` is in **failed** state
- MariaDB logs show **InnoDB storage engine errors**
- Root cause identified as **incorrect file ownership and permissions**

---

## 🔎 Root Cause Analysis

MariaDB logs showed the following errors:

```
[ERROR] InnoDB: Operating system error number 13 in a file operation
[ERROR] InnoDB: The error means mysqld does not have the access rights to the directory
[ERROR] Cannot open datafile './ibtmp1'
[ERROR] Unknown/unsupported storage engine: InnoDB
```

➡️ These errors indicate that the **mysqld process does not have permission** to access the MariaDB data directory.

---

## 🛠️ Steps to Fix the Issue

### 1️⃣ Login to the Database Server

```bash
ssh peter@stdb01
```

### 2️⃣ Check MariaDB Service Status

```bash
systemctl status mariadb.service
```

### 3️⃣ Examine MariaDB Logs

```bash
sudo tail -n 50 /var/log/mariadb/mariadb.log
```

### 4️⃣ Correct Directory Ownership

```bash
sudo chown -R mysql:mysql /var/lib/mysql
```

### 5️⃣ Set Correct Permissions

```bash
sudo chmod -R 750 /var/lib/mysql
```

### 6️⃣ Check SELinux (If Applicable)

```bash
sudo setenforce 0
```

### 7️⃣ Start MariaDB Service

```bash
sudo systemctl start mariadb
```

### 8️⃣ Verify Service Status

```bash
systemctl status mariadb
```

Expected result:

```
Active: active (running)
Status: "Taking your SQL requests now..."
```

---

## 📘 Good to Know

### 🗄️ What is MariaDB?

MariaDB is an open-source **Relational Database Management System (RDBMS)** and a drop-in replacement for MySQL.

Common uses:
- Stores and manages application data
- Used by web applications and APIs
- Supports SQL, transactions, indexing, and ACID compliance
- Common in **LAMP / LEMP stacks**

---

## 🔍 Troubleshooting Reference

### 📂 Important Paths

| Component | Path |
|--------|------|
| Logs | /var/log/mariadb/mariadb.log |
| Data Directory | /var/lib/mysql |
| Config Files | /etc/my.cnf |
| Socket File | /var/lib/mysql/mysql.sock |

---

### 🔐 Recommended Permissions

| Component | Owner | Permissions |
|--------|------|-------------|
| Data Directory | mysql:mysql | 750 |
| Config Files | root:root | 644 |
| Socket File | mysql:mysql | 660 |
| Log Files | mysql:mysql | 640 |

---

## 🏁 Conclusion

The MariaDB outage was caused by **incorrect permissions** on the data directory, preventing **InnoDB from initializing**.

Fixing ownership and permissions restored:
- InnoDB engine
- MariaDB service
- Application connectivity

✔️ MariaDB is healthy and accepting connections.
