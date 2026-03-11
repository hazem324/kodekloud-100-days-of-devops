# Day 68 - Set Up Jenkins Server

## Task Description

Install and configure Jenkins on the designated server using `yum`, start the service, and create the required administrative user.

## Requirements

### Server Access

* Connect to the Jenkins server from the jump host.
* Use:

```id="j3k2lp"
User: root
Password: S3curePass
```

---

### Jenkins Installation

* Install Jenkins using:

```id="k9s4vd"
yum
```

* Start and enable the Jenkins service.
* Resolve any timeout issues encountered during service startup.

---

### Admin User Configuration

* Access Jenkins UI after installation.
* Create an administrative user with the following details:

```id="q2x8am"
Username: theadmin
Password: Adm!n321
Full Name: Anita
Email: anita@jenkins.stratos.xfusioncorp.com
```

---

## Expected Result

* Jenkins is successfully installed using `yum`.
* Jenkins service is running without errors.
* Admin user `theadmin` is created with the specified credentials.
* Jenkins UI is accessible and fully operational.

End of Task
