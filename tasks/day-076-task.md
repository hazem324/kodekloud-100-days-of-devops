# Day 76 - Jenkins Project Security

## Task Description

Grant specific permissions to two existing Jenkins users for accessing and managing an existing job.

## Requirements

### Access Jenkins

* Open Jenkins from the top navigation.
* Login using:

```
Username: admin
Password: Adm!n321
```

---

### Target Job

* Existing job:

```
Packages
```

---

### Users

* User 1:

```
sam
Password: sam@pass12345
```

* User 2:

```
rohan
Password: rohan@pass12345
```

---

### Permission Configuration

#### Inheritance

* Enable:

```
Inherit permissions from parent ACL
```

---

#### Permissions for `sam`

Grant the following permissions:

```
Build
Configure
Read
```

---

#### Permissions for `rohan`

Grant the following permissions:

```
Build
Cancel
Configure
Read
Update
Tag
```

---

### Restrictions

* Do not modify any other configuration of the `Packages` job.

### Plugin Requirement

* Install any required authorization plugin if needed.
* Restart Jenkins if prompted after plugin installation.

### Documentation

* Capture screenshots or record the configuration process for verification.

## Expected Result

* Users `sam` and `rohan` retain their existing accounts.
* Job `Packages` permissions are configured correctly.
* `sam` has build, configure, and read access.
* `rohan` has build, cancel, configure, read, update, and tag access.
* Other job configurations remain unchanged.

End of Task
