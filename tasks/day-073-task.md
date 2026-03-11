# Day 73 - Jenkins Job for Apache Log Collection

## Task Description

Create a Jenkins job that periodically collects Apache server logs from an application server and stores them on the storage server for troubleshooting and analysis.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

### Job Creation

* Create a Jenkins job named:

```
copy-logs
```

### Scheduling

* Configure the job to build periodically.
* Use a cron schedule to run every:

```
9 minutes
```

### Log Collection

* Copy Apache logs from App Server 1 using the default Apache log directory:

```
/var/log/httpd
```

* Logs to collect:

```
access_log
error_log
```

### Destination

* Store the copied logs on the Storage Server at:

```
/usr/src/sysops
```

### Plugin Requirement

* Install any required plugins if prompted.
* Restart Jenkins if required after plugin installation.

### Documentation

* Capture screenshots or screen recordings of the configuration steps for verification and review.

## Expected Result

* Jenkins job `copy-logs` is created.
* The job runs automatically every 9 minutes.
* Apache `access_log` and `error_log` files are copied from App Server 1.
* Logs are stored on the Storage Server under `/usr/src/sysops`.
* The job executes successfully on repeated runs.

End of Task
