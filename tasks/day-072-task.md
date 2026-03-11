# Day 72 - Jenkins Parameterized Job Setup

## Task Description

Create and execute a simple parameterized Jenkins job to demonstrate how build parameters work.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```id="zv4eol"
Username: admin
Password: Adm!n321
```

---

### Job Creation

* Create a new Jenkins job named:

```id="k68xrd"
parameterized-job
```

---

### Parameters

1. **String Parameter**

* Name:

```id="w87txq"
Stage
```

* Default value:

```id="frd67x"
Build
```

2. **Choice Parameter**

* Name:

```id="s9hn5q"
env
```

* Choices:

```id="owz2le"
Development
Staging
Production
```

---

### Build Step

* Configure the job to execute a shell command that prints both parameter values passed to the job.

---

### Validation

* Run the job at least once with the parameter:

```id="i6kkdu"
env = Staging
```

* Confirm the build completes successfully.

---

### Plugin Requirement

* Install any required plugins if prompted.
* Restart Jenkins when installation completes and no jobs are running.

---

### Documentation

* Capture screenshots or record the process for verification and documentation.

## Expected Result

* Jenkins job `parameterized-job` exists and accepts parameters.
* The job prints both `Stage` and `env` parameter values during execution.
* The build completes successfully when run with `env` set to `Staging`.

End of Task
