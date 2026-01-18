# 🐘 Install and Configure PostgreSQL

The Nautilus application development team is planning to deploy a newly developed application on **Nautilus infrastructure in Stratos DC**.
The application uses **PostgreSQL** as its database backend.

The PostgreSQL database server is **already installed** on the Nautilus database server.
This task focuses on **creating a database user and database**, without restarting the PostgreSQL service.

---

##  Steps

### 1. Check PostgreSQL Service Status

Verify the current status of the PostgreSQL service:
```bash
sudo systemctl status postgresql
```
* Confirms whether PostgreSQL is running
* Shows if the service is enabled or disabled at boot

[![PostgreSQL Service Disabled](../screenshots/Screenshot-day-17-postgreSQL-service-disabled.png)](../screenshots/Screenshot-day-17-postgreSQL-service-disabled.png)

#### Enable PostgreSQL Service

Enable PostgreSQL to start automatically on system boot:
```bash
sudo systemctl enable postgresql
```
* Creates a systemd symlink
* Ensures PostgreSQL starts automatically after reboot

#### Verify Service Is Enabled

Recheck the service status:
```bash
sudo systemctl status postgresql
```

[![PostgreSQL Service Enabled](../screenshots/Screenshot-day-17-postgreSQL-service-enabled.png)](../screenshots/Screenshot-day-17-postgreSQL-service-enabled.png)

### 2. Login to the Database Server

```bash
ssh user@db_host
```

### 3. Switch to PostgreSQL System User

```bash
sudo -i -u postgres
```
[![connect to PostgreSQL System User](../screenshots/Screenshot-day-17-connect-to-postgreSQL-system-user.png)](../screenshots/Screenshot-day-17-connect-to-postgreSQL-system-user.png)

### 4. Open PostgreSQL Interactive Shell

```bash
psql
```

All database commands are executed **inside the PostgreSQL prompt**.

### 5. Create Database User

```sql
CREATE USER database_user WITH PASSWORD 'your_password';
```

📌 **Explanation**

* Creates a PostgreSQL role with login access
* This user will be used by the application to connect to the database

[![postgres-create-user](../screenshots/Screenshot-day-17-postgres-create-user.png)](../screenshots/Screenshot-day-17-postgres-create-user.png)

### 6. Create Application Database

```sql
CREATE DATABASE database_name;
```

📌 **Explanation**

* Creates a dedicated database for the application
* The database is empty and ready for use

### 7. Grant Full Privileges on Database

```sql
GRANT ALL PRIVILEGES ON DATABASE database_name TO database_user;
```

📌 **Explanation**

* Grants full access to the database for the application user
* Allows creating, modifying, and managing database objects

[![PostgreSQL Grant Privileges](../screenshots/Screenshot-day-17-postgreSQL-grant-privileges.png)](../screenshots/Screenshot-day-17-postgreSQL-grant-privileges.png)

### 8. Exit PostgreSQL

```sql
\q
```

Then exit the postgres user session:

```bash
exit
```

 **PostgreSQL service was NOT restarted**, as required.

---

## 🧠 Good to Know

### PostgreSQL Roles

* PostgreSQL uses **roles** instead of separate users and groups
* A role with `LOGIN` permission acts as a database user

### Database Privileges

* `GRANT ALL PRIVILEGES` provides full control over the database
* Permissions can be restricted later for better security

### Best Practices

* Use one database user per application
* Never expose real passwords in documentation
* Follow the principle of least privilege in production

