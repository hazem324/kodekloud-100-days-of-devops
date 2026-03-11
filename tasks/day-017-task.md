# Day 17 - Install and Configure PostgreSQL

## Task Description

The Nautilus team needs to prepare a PostgreSQL database environment to support a new application deployment on the Stratos DC infrastructure.

---

## Requirements

### 1. Database User Setup

* Create a PostgreSQL user:

```
kodekloud_top
```

* Set the password to:

```
8FmzjvFU6S
```

---

### 2. Database Creation

* Create a database:

```
kodekloud_db10
```

* Grant full privileges on this database to:

```
kodekloud_top
```

---

## Expected Result

* User `kodekloud_top` exists with the correct password.
* Database `kodekloud_db10` exists.
* The user has full access to the database.

---

## Notes

* Do not restart the PostgreSQL service.

---

End of Task
