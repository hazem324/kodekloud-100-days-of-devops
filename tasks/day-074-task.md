# Day 74 - Jenkins Database Backup Job

## Task Description

Create a Jenkins job to automate periodic backups of a MySQL database and store the backup files on a backup server.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```id="e1w1dp"
Username: admin
Password: Adm!n321
```

---

### Job Creation

* Create a Jenkins job named:

```id="e7j6h5"
database-backup
```

---

### Database Dump

* Database name:

```id="g1p3vt"
kodekloud_db01
```

* Database user:

```id="h7moxf"
kodekloud_roy
```

* Password:

```id="s8l6ra"
asdfgdsd
```

* Dump file name format:

```id="q8hrr1"
db_$(date +%F).sql
```

---

### Backup Destination

* Copy the generated dump file to the Backup Server location:

```id="bx5nrf"
/home/clint/db_backups
```

---

### Scheduling

* Configure the job to build periodically using:

```id="o8y5bn"
*/10 * * * *
```

---

### Plugin Requirement

* Install any required plugins if prompted.
* Restart Jenkins if necessary after plugin installation.

---

### Documentation

* Capture screenshots or record the configuration process for verification.

## Expected Result

* Jenkins job `database-backup` is created successfully.
* Database `kodekloud_db01` is dumped with the correct filename format.
* Backup file is copied to `/home/clint/db_backups` on the Backup Server.
* The job runs automatically every 10 minutes.
* The job executes successfully on repeated runs.

End of Task
