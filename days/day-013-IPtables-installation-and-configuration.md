# 🔐 IPtables Installation and Configuration – Apache Port Protection

## 📌 Task Overview

A website is running on the **Nautilus infrastructure** in **Stratos DC**.
The security team identified that **Apache port `6400`** is open to everyone because no firewall was configured.

To secure the application servers, **iptables** was chosen as the firewall solution.

---

## 🎯 Requirements

1. Install **iptables** and required services on app servers
2. Allow access to **port 6400 only from the LBR host**
3. Block access to **port 6400 from all other sources**
4. Ensure firewall rules **persist after reboot**


## 🛠️ Implementation (Commands shown for ONE server)

> ⚠️ **The same commands are reused on all other app servers**

### 1- Install iptables and services

```bash
sudo yum install -y iptables-services
```

**What this does:**
Installs iptables and the service required to load rules at boot.



### 2- Enable and start iptables service

```bash
sudo systemctl enable iptables
sudo systemctl start iptables
```

**What this does:**

* Enables firewall rules on system startup
* Loads iptables rules immediately


### 3- Allow LBR host to access Apache (port 6400)

```bash
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 6400 -j ACCEPT
```

**What this does:**
Allows **only the Load Balancer (LBR)** to access Apache.


### 4- Block port 6400 for everyone else

```bash
sudo iptables -A INPUT -p tcp --dport 6400 -j DROP
```

**What this does:**
Silently blocks all other traffic to port 6400.


### 5- Reject all remaining traffic (default protection)

```bash
sudo iptables -A INPUT -j REJECT --reject-with icmp-host-prohibited
```

**What this does:**
Rejects all other unwanted incoming connections.


### 6- Save firewall rules (persistence)

```bash
sudo service iptables save
```

**What this does:**
Ensures firewall rules **remain after system reboot**.


## ❌ Errors Faced & How They Were Fixed

### ❌ Error 1: Command not found

```bash
sudo iptable -L -n
```

**Error:**

```text
sudo: iptable: command not found
```

 **Fix:**

```bash
sudo iptables -L -n
```

 *iptables is plural, not singular.*


###  Error 2: Port still blocked for LBR

**Cause:**
A global `REJECT all` rule was placed **before** port 6400 rules.

**Why this is wrong:**
iptables processes rules **top → bottom**, first match wins.

 **Fix:**
Reorder rules so that:

1. LBR is allowed
2. Others are dropped
3. Everything else is rejected


##  Final Correct Rule Order

```text
ACCEPT  LBR → port 6400
DROP    everyone else → port 6400
REJECT  all other traffic
```

---

##  Verification

```bash
sudo iptables -L -n --line-numbers
```

Expected output (important part):

```text
ACCEPT tcp -- 172.16.238.14 0.0.0.0/0 tcp dpt:6400
DROP   tcp -- 0.0.0.0/0     0.0.0.0/0 tcp dpt:6400
REJECT all -- 0.0.0.0/0     0.0.0.0/0
```

---

## 🧠 Good to Know

### 🔹 What is iptables?

**iptables** is a **Linux firewall tool** used to:

* Control incoming and outgoing network traffic
* Protect servers from unauthorized access
* Define security rules at the kernel level


### 🔹 Important Files

* `/etc/sysconfig/iptables` → Saved firewall rules
* `/usr/lib/systemd/system/iptables.service` → Service definition


### 🔹 Core Concepts

* **Tables**: `filter` (default), `nat`, `mangle`
* **Chains**:

  * `INPUT` → incoming traffic
  * `OUTPUT` → outgoing traffic
  * `FORWARD` → routed traffic
* **Targets**:

  * `ACCEPT` → allow
  * `DROP` → silently block
  * `REJECT` → block with response


### 🔹 Rule Processing Logic

* Rules are evaluated **top to bottom**
* **First match wins**
* Rules after a `DROP` or `REJECT` may never execute


### 🔹 Common Commands

```bash
iptables -L -n            # list rules
iptables -A               # append rule
iptables -I               # insert rule at position
iptables -D               # delete rule
service iptables save     # persist rules
```


##  Final Status

*  Apache port secured
*  Only LBR can access port 6400
*  Firewall rules persistent
*  Same steps reused across all app servers

---
