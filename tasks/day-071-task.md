# Day 71 - Configure Jenkins Job for Package Installation

## Task Description

Create a Jenkins job that installs a user-specified package on the storage server using a parameterized build.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

### Jenkins Job

* Create a job with the name:

```
install-packages
```

### Parameter Configuration

* Add a string parameter:

```
Name: PACKAGE
```

* This parameter will define the package name to be installed during the build.

### Job Configuration

* Configure the job to install the package specified by:

```
$PACKAGE
```

* The installation must be executed on the storage server within the Stratos Datacenter.

### Plugin Requirement

* Install any required plugins if prompted.
* Restart Jenkins when installation is complete and no jobs are running.

### Validation

* Run the Jenkins job multiple times with different package values to confirm reliable execution.

### Documentation

* Capture screenshots or record the configuration steps for review and documentation.

## Expected Result

* Jenkins job `install-packages` is created successfully.
* Job accepts a parameter named `PACKAGE`.
* The specified package installs on the storage server when the job runs.
* Job executes successfully on repeated runs.

End of Task
