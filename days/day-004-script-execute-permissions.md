# Script Execution Permissions

---

##  Objective

The xFusionCorp Industries sysadmin team created a Bash script named `xfusioncorp.sh` to automate backup processes.  
Although the script is present on the server, it does not have executable permissions.

**Goal**:
- Grant executable permissions to `/tmp/xfusioncorp.sh`
- Ensure **all users** can execute the script

---

##  Steps 

### 1. Connect to App Server 2

```bash
ssh steve@172.16.238.11
```

---

### 2. Check Current File Permissions

```bash
ls -al /tmp/
```

**Output:**

```text
---------- 1 root root 40 Dec 23 10:24 xfusioncorp.sh
```

➡️ The file has **no permissions at all**, meaning no user can read or execute it.

---

### 3. Grant Execute Permissions for All Users

```bash
sudo chmod 755 /tmp/xfusioncorp.sh
```

**Explanation**:

* `7` → Owner: read, write, execute (`rwx`)
* `5` → Group: read, execute (`r-x`)
* `5` → Others: read, execute (`r-x`)

This ensures **every user** can execute the script.

---

### 4. Verify the Changes

```bash
ls -al /tmp/
```

**Output:**

```text
-rwxr-xr-x 1 root root 40 Dec 23 10:24 xfusioncorp.sh
```

 The script is now executable by all users.

---

## 📌 Why `chmod 755` Is Required for Scripts

Unlike compiled binaries, shell scripts must be **readable** by the interpreter (`/bin/bash` or `/bin/sh`).

### 🔍 Key Reasons:

* **Interpreter Needs Read Access**
  The shell must read the script to execute its commands.
* **Execute Only Is Not Enough**
  Permissions like `--x--x--x` will cause a *Permission denied* error.
* **Industry Standard**
  `755` is the recommended permission for shared executable scripts.

---

##  Understanding `chmod`

### Permission Values

| Permission | Symbol | Value |
| ---------- | ------ | ----- |
| Read       | r      | 4     |
| Write      | w      | 2     |
| Execute    | x      | 1     |

### User Categories

| Category     | Symbol |
| ------------ | ------ |
| User (Owner) | u      |
| Group        | g      |
| Others       | o      |

---

## Numeric (Octal) Mode Example

```bash
chmod 755 xfusioncorp.sh
```

Breakdown:

* `7` → rwx (User)
* `5` → r-x (Group)
* `5` → r-x (Others)

---

##  Symbolic Mode Example

```bash
chmod u=rwx,g=rx,o=rx xfusioncorp.sh
```

---

##  Common Permission Patterns

| Mode | Meaning                          |
| ---- | -------------------------------- |
| 755  | Executable script (recommended)  |
| 644  | Regular file                     |
| 600  | Private file                     |
| 777  | Full access (⚠️ not recommended) |

---

##  Security Notes

* Follow the **Principle of Least Privilege**
* Avoid `777` unless absolutely necessary
* `/tmp` uses the **sticky bit** to prevent users from deleting others’ files

