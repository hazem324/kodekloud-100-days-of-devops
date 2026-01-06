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


### Install MariaDB server:

```bash
sudo yum install -y mariadb-server
```


### 3️⃣ Enable and Start MariaDB (DB Server)

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

📸 **Screenshot: MariaDB Service Running on DB Server**

---

### 4️⃣ Access MariaDB as Root

```bash
sudo mariadb -u root
```

➡ Logs into MariaDB using root via Unix socket authentication.

📸 **Screenshot: MariaDB Root Login Successful**

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

📸 **Screenshot: Database and User Created Successfully**

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

📸 **Screenshot: Database and User Verification**

---

### 7️⃣ Final Application Test via Load Balancer

* Open the **LBR URL**
* Click **App** from the top menu

✅ Expected message:

```
App is able to connect to the database using user kodekloud_top
```

📸 **Screenshot: Final Application Connectivity Test**

---

## 🐬 MySQL / MariaDB Cheat Sheet

| Command                                    | Purpose               |
| ------------------------------------------ | --------------------- |
| `sudo mariadb -u root`                     | Login as MariaDB root |
| `SHOW DATABASES;`                          | List all databases    |
| `USE db_name;`                             | Select a database     |
| `CREATE DATABASE db;`                      | Create a new database |
| `CREATE USER 'u'@'%' IDENTIFIED BY 'p';`   | Create a user         |
| `GRANT ALL PRIVILEGES ON db.* TO 'u'@'%';` | Grant permissions     |
| `FLUSH PRIVILEGES;`                        | Reload permissions    |
| `SELECT User, Host FROM mysql.user;`       | List users            |
| `EXIT;`                                    | Exit MariaDB shell    |

---

## ℹ️ Good to Know

* **Port 3002**
  Used for internal application traffic behind the Load Balancer to avoid conflicts with default ports.

* **Shared Storage**
  Ensures all App Servers serve the same WordPress files, enabling load balancing.

* **MariaDB Unix Socket Authentication**
  Allows root login without a password when using `sudo`, increasing local security.

* **Dedicated DB User**
  Follows the principle of least privilege and protects the root account.

* **Load Balancer Validation**
  Confirms the entire stack (Web + PHP + DB + Network) is working correctly.

---

✅ **Task Status: Successfully Completed**
The WordPress application is fully operational and connected to the database through the Load Balancer.
