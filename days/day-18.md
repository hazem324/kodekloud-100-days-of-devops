# 🌐 WordPress Infrastructure Setup – Stratos Datacenter

xFusionCorp Industries deployed a multi-tier infrastructure to host a WordPress website.
The shared directory `/vaw/www/html` is already mounted on all App Servers under `/var/www/html`.

This task ensures:

* Web servers are correctly configured
* Database access is secure and functional
* Application connectivity works through the Load Balancer (LBR)

---

## 🛠️ Fix Steps (Corrected & Complete)

### 1️⃣ Install Apache, PHP, and Dependencies (All App Servers)

Run the following command **on each App Server**:

```bash
sudo yum install -y httpd php php-mysqlnd php-gd php-xml php-mbstring
```

**One-line explanation:**
➡ Installs Apache web server, PHP runtime, and required PHP extensions for WordPress.

🔹 **What each package does:**

* **httpd** → Apache web server to handle HTTP requests
* **php** → Executes WordPress PHP code
* **php-mysqlnd** → Allows PHP to communicate with MariaDB
* **php-gd** → Image processing (media uploads, thumbnails)
* **php-xml** → XML handling used by WordPress
* **php-mbstring** → Handles multi-byte characters (UTF-8)

---

### 2️⃣ Enable Apache Service

```bash
sudo systemctl enable httpd
```

➡ Ensures Apache starts automatically after reboot.

---

### 3️⃣ Configure Apache to Listen on Port 3002

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

➡ Opens Apache main configuration file.

```apache
Listen 3002
```
[![Configure Apache to Listen on Port 3002](../screenshots/Screenshot-day-18-config-httpd-port.png)](../screenshots/Screenshot-day-18-config-httpd-port.png)

➡ Changes Apache listening port from default to **3002**.

---

### 4️⃣ Restart and Verify Apache

```bash
sudo systemctl restart httpd
```

➡ Applies the new Apache configuration.

```bash
sudo systemctl status httpd
```

➡ Confirms Apache is running and listening on port **3002**.

[![httpd server running](../screenshots/Screenshot-day-18-httpd-service-running.png)](../screenshots/Screenshot-day-18-httpd-service-running.png)

### Install MariaDB server (on Database Server):

```bash
sudo yum install -y mariadb-server
```


### 3️⃣ Enable and Start MariaDB 

```bash
sudo systemctl enable mariadb
```

➡ Starts MariaDB automatically on system boot.

```bash
sudo systemctl start mariadb
```

➡ Launches the MariaDB service.

```bash
sudo systemctl status mariadb
```

➡ Confirms MariaDB is running successfully.

[![MariaDB Service Running on DB Server](../screenshots/Screenshot-day-18-mariaDB-service-running-on-DB-server.png)](../screenshots/Screenshot-day-18-mariaDB-service-running-on-DB-server.png)

---

### 4️⃣ Access MariaDB as Root

```bash
sudo mariadb -u root
```

➡ Logs into MariaDB using root via Unix socket authentication.

---

### 5️⃣ Create Database and User

```sql
CREATE DATABASE kodekloud_db2;
```

➡ Creates the application database.

```sql
CREATE USER 'kodekloud_top'@'%' IDENTIFIED BY 'YchZHRcLkL';
```

➡ Creates a database user accessible from App Servers.

```sql
GRANT ALL PRIVILEGES ON kodekloud_db2.* TO 'kodekloud_top'@'%';
```

➡ Gives full access on the database to the application user.

```sql
FLUSH PRIVILEGES;
```

➡ Reloads permission tables immediately.


[![Database and User Created Successfully](../screenshots/Screenshot-day-18-database-and-user-created-successfully.png)](../screenshots/Screenshot-day-18-database-and-user-created-successfully.png)

---

### 6️⃣ Verify Database Configuration

```sql
SHOW DATABASES;
```

➡ Confirms the database exists.

```sql
SELECT User, Host FROM mysql.user;
```

➡ Confirms user and remote access permissions.

[![Database and User Verification](../screenshots/Screenshot-day-18-database-and-user-verification.png)](../screenshots/Screenshot-day-18-database-and-user-verification.png)

---

### 7️⃣ Final Application Test via Load Balancer

* Open the **LBR URL**
* Click **App** from the top menu

✅ Expected message:

```
App is able to connect to the database using user kodekloud_top
```

[![Final Application Connectivity Test](../screenshots/Screenshot-day-18-final-application-connectivity-test.png)](../screenshots/Screenshot-day-18-final-application-connectivity-test.png)

---

# 🐘 PostgreSQL Cheat Sheet

Help with SQL commands to interact with a PostgreSQL database

---

## PostgreSQL Locations

Linux:

```
/usr/bin/psql
```

Data directory (varies by distro):

```
/var/lib/pgsql/data
/var/lib/postgresql/data
```

---

## Login

Login as postgres user:

```bash
sudo -i -u postgres
psql
```

Login to a specific database:

```bash
psql -d database_name
```

Login with user:

```bash
psql -U database_user -d database_name
```

Exit PostgreSQL:

```sql
\q
```

---

## Users & Roles

Show users (roles):

```sql
\du
```

Create user:

```sql
CREATE USER database_user WITH PASSWORD 'your_password';
```

Grant login permission:

```sql
ALTER USER database_user WITH LOGIN;
```

Delete user:

```sql
DROP USER database_user;
```

Show role privileges:

```sql
SELECT rolname FROM pg_roles;
```

---

## Databases

Show databases:

```sql
\l
```

Create database:

```sql
CREATE DATABASE database_name;
```

Delete database:

```sql
DROP DATABASE database_name;
```

Connect to database:

```sql
\c database_name
```

Grant all privileges on database:

```sql
GRANT ALL PRIVILEGES ON DATABASE database_name TO database_user;
```

---

## Tables

Show tables:

```sql
\dt
```

Create table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(50),
    password VARCHAR(100),
    location VARCHAR(100),
    dept VARCHAR(100),
    is_admin BOOLEAN,
    register_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Drop table:

```sql
DROP TABLE users;
```

Describe table:

```sql
\d users
```

---

## Insert Data

Insert row:

```sql
INSERT INTO users (first_name, last_name, email)
VALUES ('Brad', 'Traversy', 'brad@gmail.com');
```

Insert multiple rows:

```sql
INSERT INTO users (first_name, last_name, email)
VALUES
('Fred', 'Smith', 'fred@gmail.com'),
('Sara', 'Watson', 'sara@gmail.com');
```

---

## Select Queries

Select all:

```sql
SELECT * FROM users;
```

Select specific columns:

```sql
SELECT first_name, last_name FROM users;
```

Where clause:

```sql
SELECT * FROM users WHERE location = 'Massachusetts';
```

AND condition:

```sql
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
```

---

## Update & Delete

Update row:

```sql
UPDATE users SET email='new@mail.com' WHERE id=2;
```

Delete row:

```sql
DELETE FROM users WHERE id=6;
```

---

## Alter Table

Add column:

```sql
ALTER TABLE users ADD age INT;
```

Modify column:

```sql
ALTER TABLE users ALTER COLUMN age TYPE VARCHAR(3);
```

Drop column:

```sql
ALTER TABLE users DROP COLUMN age;
```

---

## Sorting & Filtering

Order by:

```sql
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

Distinct:

```sql
SELECT DISTINCT location FROM users;
```

Between:

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```

Like:

```sql
SELECT * FROM users WHERE dept LIKE 'dev%';
```

IN:

```sql
SELECT * FROM users WHERE dept IN ('design', 'sales');
```

---

## Joins

Inner Join:

```sql
SELECT users.first_name, posts.title
FROM users
INNER JOIN posts ON users.id = posts.user_id;
```

Left Join:

```sql
SELECT comments.body, posts.title
FROM comments
LEFT JOIN posts ON posts.id = comments.post_id;
```

Multiple Joins:

```sql
SELECT users.first_name, posts.title, comments.body
FROM comments
INNER JOIN posts ON posts.id = comments.post_id
INNER JOIN users ON users.id = comments.user_id;
```

---

## Indexes

Create index:

```sql
CREATE INDEX idx_location ON users(location);
```

Drop index:

```sql
DROP INDEX idx_location;
```

---

## Aggregate Functions

Count:

```sql
SELECT COUNT(*) FROM users;
```

Max / Min / Sum:

```sql
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
```

Group By:

```sql
SELECT age, COUNT(age) FROM users GROUP BY age;
```

Having:

```sql
SELECT age, COUNT(age)
FROM users
GROUP BY age
HAVING COUNT(age) >= 2;
```

---

* PostgreSQL uses **roles**, not separate users/groups
* `SERIAL` auto-increments IDs
* `BOOLEAN` replaces `TINYINT(1)`
* `TIMESTAMP` replaces `DATETIME`
* Case-sensitive strings use **single quotes**

##  Good to Know?

###  LAMP Stack Architecture

* **Linux**: Operating system providing stability and security
* **Apache**: Web server that receives and serves HTTP requests
* **MariaDB (MySQL)**: Relational database storing application data
* **PHP**: Server-side language that runs WordPress logic

---

###  Web Application Request Flow

1. **Client Request**
   Browser sends an HTTP request to the Load Balancer.
2. **Load Balancer Routing**
   Request is forwarded to one of the App Servers.
3. **Apache Processing**
   Apache receives the request on port **3002**.
4. **PHP Execution**
   Apache passes PHP files to the PHP interpreter.
5. **Database Query**
   PHP connects to MariaDB to fetch or store data.
6. **Response to Client**
   Generated HTML is returned to the browser.

---

###  Load Balancer Role

* Distributes traffic across multiple App Servers
* Prevents overloading a single server
* Improves **availability** and **scalability**
* Acts as the only public entry point

---

###  Shared Storage Importance

* Ensures all App Servers serve the same WordPress files
* Prevents file inconsistency between servers
* Simplifies updates and maintenance
* Critical for load-balanced environments

---

###  Database Security Best Practices

* **Remote Access**
  `'user'@'%'` → Allows connections from any host (used in this task)
* **Local Only Access**
  `'user'@'localhost'` → Most secure, DB-only applications
* **Restricted Network Access**
  `'user'@'192.168.1.%'` → Allows a specific subnet
* **Avoid Root for Applications**
  Always use a dedicated DB user

---

###  Why Unix Socket Authentication Is Used

* Root DB access allowed only via OS root user
* No password exposed or transmitted
* More secure than password-based root login
* Common in enterprise Linux environments

---

###  Performance Optimization Concepts

* **Connection Pooling**
  Reuse database connections to reduce overhead
* **Query Optimization**
  Use indexes to speed up data retrieval
* **Caching**
  Use Redis or Memcached to reduce DB load
* **Load Balancing**
  Spread traffic across servers for better response time

---

### 🔹 DevOps Best Practices Demonstrated

* Separation of concerns (Web vs DB)
* Least privilege access
* High availability design
* Scalable infrastructure
* Production-ready architecture

---