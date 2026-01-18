#  Install and Configure Tomcat Server

##  Task Overview

The Nautilus application development team has completed the beta version of a Java-based application and decided to deploy it using **Apache Tomcat**.

The application must be deployed on **App Server 3 (stapp03)**, run on **port 8088**, and be accessible directly via the **base URL**.

```bash
curl http://stapp03:8088
```

---

##  Objectives

- Install Java (JVM)
- Install and configure Apache Tomcat
- Run Tomcat on port **8088**
- Deploy `ROOT.war` from Jump Host
- Ensure application loads at base URL `/`

---

##  Steps

### 1. Install Java (JVM)

Tomcat is a Java application and requires a JVM.

```bash
sudo yum install java-1.8.0-openjdk-devel -y
```

### 2. Create a Dedicated Tomcat User (Best Practice)

Running Tomcat as root is insecure.  
A dedicated user improves security and follows the principle of least privilege.

```bash
sudo groupadd tomcat
sudo useradd -M -U -d /opt/tomcat -s /sbin/nologin -g tomcat tomcat
```

**Why this is important:**
- Prevents interactive login
- Limits damage if Tomcat is compromised
- Isolates Tomcat from system users

### 3. Create Tomcat Installation Directory

```bash
sudo mkdir /opt/tomcat
```

This directory will hold the Tomcat runtime files.

### 4. Download and Extract Apache Tomcat

```bash
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz
```

Extract into `/opt/tomcat`:

```bash
sudo tar -xf apache-tomcat-9.0.80.tar.gz -C /opt/tomcat --strip-components=1
```

### 5. Set Correct Permissions

Tomcat must own its own files.

```bash
sudo chown -R tomcat:tomcat /opt/tomcat
```

### 6. Create a systemd Service for Tomcat

Creating a service allows Tomcat to:
- Start at boot
- Restart automatically on failure
- Be managed using `systemctl`

```bash
sudo vi /etc/systemd/system/tomcat.service
```

Add the following content:

```ini
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat

Environment="JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b09-4.el9.x86_64"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### 7- Enable and Start Tomcat

```bash
sudo systemctl daemon-reload
sudo systemctl enable tomcat
sudo systemctl start tomcat
```

Verify status:

```bash
systemctl status tomcat
```

### 8- (Optional) Firewall Configuration

This step may be skipped if firewall is disabled.

```bash
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --reload
```

### 9- Modify Tomcat Port to 8088

Edit Tomcat configuration:

```bash
vi /opt/tomcat/conf/server.xml
```

Update the HTTP connector:

```xml
<Connector port="8088"
           protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443"
           maxParameterCount="1000" />
```

Restart Tomcat:

```bash
systemctl restart tomcat
```

### 10- Backup Default ROOT Application

```bash
cd /opt/tomcat/webapps
mv ROOT ROOT.bak
```

### 11- Copy ROOT.war from Jump Host

From Jump Host:

```bash
scp /tmp/ROOT.war banner@stapp03:/home/tony/
```

### 12- Deploy ROOT Application

Extract the application into the ROOT context:

```bash
unzip /home/tony/ROOT.war -d /opt/tomcat/webapps/ROOT
```

Ensure correct ownership:

```bash
sudo chown -R tomcat:tomcat /opt/tomcat/webapps/ROOT
```

Restart Tomcat:

```bash
systemctl restart tomcat
```

### 13- Verify Deployment

```bash
curl http://stapp03:8088
```

**✅ Expected Output**

The application is successfully deployed and accessible via the base URL.

Click the screenshot below to view the deployed application:

[![Tomcat Application Output](../screenshots/Screenshot_day_011_tomcat_server.png)](../screenshots/Screenshot_day_011_tomcat_server.png)

---

## 🧠 Good to Know


### What is Apache Tomcat?

Apache Tomcat is a lightweight application server used to run **Java web applications**.  
It works by serving Java applications such as **Servlets and JSP** over HTTP.

#### Why is Tomcat used?
- To run Java-based web applications
- To deploy `.war` (Web Application Archive) files
- To provide a simple and reliable web server for Java apps

Tomcat is widely used because it is **fast, open-source, easy to configure**, and suitable for both development and production environments.


### Apache Tomcat Fundamentals

- **Purpose**: Java Servlet container and web server
- **Architecture**:
  - Catalina → Servlet container
  - Coyote → HTTP connector
  - Jasper → JSP engine
- **Default Ports**:
  - 8080 → HTTP
  - 8443 → HTTPS
  - 8005 → Shutdown

### Directory Structure

- `/bin` → Startup & shutdown scripts
- `/conf` → Configuration files
- `/webapps` → Deployed applications
- `/logs` → Log files

### Java Application Deployment

- WAR file: Web Application Archive
- ROOT application: Served at `/`
- Context path: URL path of the app
- Hot deployment: Deploy apps without stopping server

### Tomcat Configuration Files

- `server.xml` → Core server configuration
- `web.xml` → Application descriptor
- `tomcat-users.xml` → Authentication
- `catalina.properties` → System properties

### Security Best Practices

- Run Tomcat as a non-root user
- Restrict file permissions
- Remove or secure default apps
- Enable HTTPS (SSL/TLS) in production

---

## ✅ Final Result

-  Tomcat installed and running
-  Port configured to 8088
-  Application deployed as ROOT
-  Accessible via base URL

```
http://stapp03:8088
```

---
