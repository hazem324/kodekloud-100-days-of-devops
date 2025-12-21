# Temporary User Setup with Expiry Date (Day 2 - Nautilus Project)

As part of your temporary assignment to the **Nautilus project**, a developer named **kareem** requires access for a limited duration. To ensure proper access management and security, we need to create a temporary user account that automatically expires after a specific date.

### Task
Create a user named **kareem** on **App Server 2** in Stratos Datacenter.  
Set the account expiry date to **2024-02-17**.  
The username must be created in **lowercase** as per standard protocol.

### Prerequisites
- You have SSH access to **App Server 2** (as user `steve` or any sudo-enabled user).
- You have sudo privileges.

### Steps

1. Connect to **App Server 2** via SSH (use the provided credentials from the infrastructure details).

2. Create the user `kareem` with the specified expiry date using the following command:

   ```bash
   sudo useradd -m -e 2024-02-17 kareem
   ```

   **Explanation of flags:**
   - `-m`: Creates the user's home directory (`/home/kareem`).
   - `-e 2024-02-17`: Sets the account expiry date to February 17, 2024.
   - `kareem`: Username in lowercase.

3. (Optional) Set a password for the user if immediate login is required:

   ```bash
   sudo passwd kareem
   ```

   > Note: In many practice environments, you may not need to set a password immediately, but it's good practice for real-world scenarios.

### Key Linux User Management Files

Understanding where user information is stored helps in verification and troubleshooting:

| File              | Purpose                                                                 | Command to View Relevant Entry                  |
|-------------------|-------------------------------------------------------------------------|-------------------------------------------------|
| `/etc/passwd`     | Stores basic user account information (username, UID, GID, home dir, shell) | `grep '^kareem' /etc/passwd`                    |
| `/etc/shadow`     | Stores secure password and **account expiry** information                | `sudo grep '^kareem' /etc/shadow`               |
| `/etc/group`      | Stores group membership information                                     | `grep kareem /etc/group`                        |
| `/etc/login.defs` | Default settings for user creation (used by useradd)                    | Not user-specific, but defines defaults         |

> **Important**: `/etc/shadow` is readable only by root — always use `sudo` to view it.

### Verification Steps

Run the following commands to confirm the user was created correctly and the expiry date is set:

1. Check the `/etc/passwd` entry:

   ```bash
   grep '^kareem' /etc/passwd
   ```

   Expected output similar to:
   ```
   kareem:x:1002:1002::/home/kareem:/bin/bash
   ```

2. Check the `/etc/shadow` entry (this shows the expiry date):

   ```bash
   sudo grep '^kareem' /etc/shadow
   ```

   Focus on the **8th field** (account expiry date in days since Jan 1, 1970). It should show a number corresponding to 2024-02-17.

3. Verify account expiry and aging information (recommended method):

   ```bash
   sudo chage -l kareem
   ```

   Look for these key lines:
   ```
   Account expires                : Feb 17, 2024
   ```

4. Test login as the new user (optional, requires password to be set):

   ```bash
   su - kareem
   ```

   Then run:
   ```bash
   whoami
   pwd
   exit
   ```

   Expected:
   - `whoami` → `kareem`
   - `pwd` → `/home/kareem`

### Test Your Work (Self-Assessment)

After completing the task, perform these checks:

| Test                          | Command                                            | Expected Result                                      |
|-------------------------------|----------------------------------------------------|------------------------------------------------------|
| User exists in passwd         | `grep '^kareem' /etc/passwd`                       | Shows entry with `/home/kareem` and `/bin/bash`      |
| Entry in shadow               | `sudo grep '^kareem' /etc/shadow`                   | Shows encrypted password and expiry field            |
| Home directory created        | `ls /home/kareem`                                  | Directory exists (may be empty)                       |
| Expiry date set correctly     | `sudo chage -l kareem \| grep "Account expires"`   | `Account expires: Feb 17, 2024`                      |
| No password expiry (default)  | `sudo chage -l kareem \| grep "Password expires"`  | `Password expires: never`                            |
| Login possible (if pwd set)   | `su - kareem`                                      | Switches to kareem's shell successfully              |

### Good to Know

- **Account Expiry Behavior**: After Feb 17, 2024, the account will be locked — login fails, but account and files remain until manually removed.
- **Extend Expiry (if needed)**:  
  ```bash
  sudo chage -E 2024-12-31 kareem
  ```
  Remove expiry:
  ```bash
  sudo chage -E -1 kareem
  ```
- **Clean Up**:  
  ```bash
  sudo userdel -r kareem
  ```

### Related Commands Summary

| Command                  | Purpose                                      |
|--------------------------|----------------------------------------------|
| `useradd -m -e DATE user`| Create user with expiry                      |
| `chage -l username`      | View account/password ageing info            |
| `usermod -e DATE user`   | Modify expiry on existing user               |
| `passwd -S username`     | Check password status                        |
| `userdel -r username`    | Completely remove user and home directory    |