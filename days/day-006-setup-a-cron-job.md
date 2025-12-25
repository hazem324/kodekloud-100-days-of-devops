# Day 6 – Setup a Cron Job on Nautilus App Servers

## 🎯 Objective

The Nautilus system admins team needs to verify that scheduled tasks can run correctly on all **Nautilus app servers** in the **Stratos Datacenter** before deploying real automation scripts.

This task validates:
- Cron service installation
- Cron daemon execution
- A scheduled task for the **root user**

---

**cronie** is the Linux package that provides the **cron scheduler**, which allows tasks (commands or scripts) to run automatically at defined times or intervals.

Cron jobs on CentOS systems are managed by the **cronie** package.

---

## 🛠️ Steps to Perform the Task

### 1️⃣ Install Cron Service (cronie)


Run the following command:

```bash
sudo yum install cronie -y
```

---

### 2️⃣ Start and Enable the Cron Daemon

Start the cron service so jobs can run immediately, and enable it to start automatically on reboot.

```bash
sudo systemctl start crond
sudo systemctl enable crond
```

Verify the service status:

```bash
systemctl status crond
```

The service should show **active (running)**.

---

### 3️⃣ Add a Cron Job for Root User

Edit the root user’s crontab:

```bash
sudo crontab -e
```

Add the following line:

```bash
*/5 * * * * echo hello > /tmp/cron_text
```

📌 This cron job:

* Runs every **5 minutes**
* Writes the word **hello** into `/tmp/cron_text`

Save and exit the editor.

---

### 4️⃣ Verify the Cron Job

List the root cron jobs to confirm the entry exists:

```bash
sudo crontab -l
```

---

### 5️⃣ Test the Cron Execution

Wait at least **5 minutes**, then check the output file:

```bash
cat /tmp/cron_text
```

Expected output:

```text
hello
```

---

## 📘 Good to Know (After This Task)

### ⏱️ Cron Schedule Format

Cron syntax uses 5 time fields:

```text
minute hour day month weekday command
```

Examples:

* `*/5 * * * *` → every 5 minutes
* `0 1 * * *` → every day at 1 AM

---

### 🧩 Cron Types

Cron jobs can be scheduled in different ways:

* **User Crontab**

  ```bash
  crontab -e
  ```

  → Used for per-user scheduled tasks

* **System Crontab**

  ```text
  /etc/crontab
  ```

  → Used for system-wide scheduling (includes user field)

* **Cron Directories**

  ```text
  /etc/cron.d/
  /etc/cron.daily/
  /etc/cron.hourly/
  /etc/cron.weekly/
  ```

  → Used for periodic system tasks

---

### ✳️ Special Characters in Cron

Cron uses special characters to define schedules:

* `*` → any value
* `,` → list (e.g. `1,3,5`)
* `-` → range (e.g. `1-5`)
* `/` → step (e.g. `*/5`)

Example:

```text
*/10 * * * * → every 10 minutes
```

---

### 👤 Root vs User Cron Jobs

* `sudo crontab -e` → system-level tasks (used here)
* `crontab -e` → user-specific tasks

Each user has a **separate crontab**.

---

### 🧪 Troubleshooting Tips

If the cron job does not run:

* Check cron logs:

  ```bash
  sudo tail -f /var/log/cron
  ```
* Ensure `crond` is running
* Verify cron syntax
* Confirm file permissions

---

### 🚀 Why This Task Is Important

This task confirms that:

* Cron is correctly installed and running
* Scheduled automation works
* The servers are ready for real DevOps scripts

It is a **foundation step** for automation across all app servers.