# Day 10 - Linux Bash Scripts

## Task Description

The xFusionCorp production support team needs a Bash script to automate backups of a static website running on App Server 1.

---

## Requirements

### 1. Script Creation

* Create a Bash script named:

```
news_backup.sh
```

* Location:

```
/scripts
```

---

### 2. Backup Operation

The script must:

* Create a ZIP archive named:

```
xfusioncorp_news.zip
```

* Archive source:

```
/var/www/html/news
```

* Store the archive locally in:

```
/backup/
```

---

### 3. Remote Copy

* Copy the archive to the **Nautilus Backup Server**.
* Destination path on backup server:

```
/backup/
```

* The script must perform the copy **without password prompts**.

---

### 4. Execution Constraints

* The App Server user (e.g., `tony`) must be able to execute the script.
* Do not use `sudo` inside the script.

---

## Expected Result

* The ZIP file is created successfully.
* The archive exists on both the App Server and the Backup Server.
* The script runs non-interactively.

---

## Notes

* The `zip` package must be installed manually before running the script.
* Backup data in `/backup/` is temporary.

---

End of Task

