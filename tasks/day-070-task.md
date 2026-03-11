# Day 70 - Configure Jenkins User Access

## Task Description

Configure Jenkins access control by creating a new user and assigning limited permissions using the Project-based Matrix Authorization Strategy.

## Requirements

### Access Jenkins

* Open Jenkins using the top navigation button.
* Login with:

```
Username: admin
Password: Adm!n321
```

### Create User

* Create a Jenkins user with the following details:

```
Username: mariyam
Password: GyQkFRVNr3
Full Name: Mariyam
```

### Authorization Strategy

* Enable:

```
Project-based Matrix Authorization Strategy
```

* Grant the following permission to:

```
User: mariyam
Permission: Overall Read
```

### Anonymous Access

* Remove all permissions assigned to:

```
Anonymous
```

* Ensure the following user retains full administrative permissions:

```
admin (Overall Administer)
```

### Job-Level Permissions

* For the existing Jenkins job:

  * Grant:

```
User: mariyam
Permission: Read
```

* Do not grant additional permissions such as Agent, SCM, Build, or others.

### Plugin Requirement

* Install any required plugin that enables matrix authorization if not already installed.
* Restart Jenkins if prompted after plugin installation.

### Documentation

* Capture screenshots or screen recordings of the configuration steps for verification and review.

## Expected Result

* User `mariyam` is successfully created.
* Project-based Matrix Authorization Strategy is enabled.
* `mariyam` has Overall Read permission only.
* `Anonymous` users have no permissions.
* The existing job allows `mariyam` read access only.
* Jenkins admin retains full administrative privileges.

End of Task
