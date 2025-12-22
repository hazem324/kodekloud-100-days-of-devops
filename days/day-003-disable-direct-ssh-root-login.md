# Disable Direct SSH Root Login on Nautilus App Servers (Stratos Datacenter)

## Overview
This document describes how to disable direct SSH root login on the Nautilus application servers in the Stratos Datacenter:

- stapp01 — Nautilus App 1  
- stapp02 — Nautilus App 2  
- stapp03 — Nautilus App 3

Disabling direct root SSH login reduces the attack surface and enforces use of individual accounts with sudo for better auditing and accountability.

---

## Important Prerequisites & Safety Checklist
- Ensure a non-root user with sudo privileges exists on each server (examples: `thor`, `steve`, `banner`).
- Log in using that sudo-enabled user when performing changes:
  ```bash
  ssh username@stapp01
  ```
  (replace `username` and the server name accordingly)
- CRITICAL WARNING: Before you modify SSH or restart the SSH service, open a *second* terminal/session and confirm you can:
  - SSH into the server as the non-root user, and
  - Use `sudo` to run a command (e.g., `sudo -v` or `sudo ls /root`).
  If either fails, fix the non-root sudo access before disabling root SSH. Disabling root without a working alternative can lock you out.

If you need to create a sudo user (example):
```bash
# create user and set password (interactive)
sudo adduser newuser

# add user to sudoers group (Debian/Ubuntu)
sudo usermod -aG sudo newuser

# or on RHEL/CentOS use "wheel"
sudo usermod -aG wheel newuser

# test sudo
su - newuser
sudo -v
```

---

## Steps to Disable Direct Root SSH Login

1) Log in as the sudo-enabled non-root user:
```bash
ssh username@stapp01
```

2) Edit the SSH daemon configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```
- Find the `PermitRootLogin` line. It may be one of:
  - `#PermitRootLogin yes`
  - `PermitRootLogin yes`
  - `PermitRootLogin prohibit-password`
- Change or add the line so it reads:
  ```
  PermitRootLogin no
  ```
- Save & exit (in nano: Ctrl+O → Enter → Ctrl+X).

Explanation:
- `PermitRootLogin` controls whether the `root` user can SSH directly.
- `no` prevents any interactive SSH login as root (both password and key-based), enforcing login via a non-root account and escalation to root with `sudo`.

3) Restart the SSH service to apply changes:
```bash
sudo systemctl restart sshd
```
(older systems may use `sudo service sshd restart`)

Explanation:
- Restarting `sshd` tells the SSH daemon to re-read `/etc/ssh/sshd_config` and apply the change.
- Doing this as the non-root sudo user is safe only after you verified sudo access in another session.

---

## Quick One-Liner Alternative (use with caution)
These `sed` commands try to replace common forms of `PermitRootLogin` with `PermitRootLogin no` and then restart `sshd`:
```bash
sudo sed -i 's/^#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/^PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

Explanation:
- sed is a command-line tool used to search and modify text inside files automatically.
- The -i option means edit the file in place, so the changes are saved directly in the file instead of just being printed on the screen.
- `sed -i 's/pattern/replacement/' file` edits the file in-place replacing the first match of `pattern` with `replacement`.
- The two sed lines handle both commented (`#PermitRootLogin yes`) and uncommented (`PermitRootLogin yes`) common variants.
- After modification, `systemctl restart sshd` reloads the SSH daemon.
- Caution: These commands assume the file contains those exact lines. If your config uses different values (e.g., `prohibit-password`) or spacing, you may need to adjust the pattern. Always inspect the file after running `sed` and before restarting.

---

## Verification (do this from a separate/new terminal)
1. Confirm non-root SSH still works:
```bash
ssh username@stapp01
sudo -v
```
2. Attempt direct root SSH (should fail):
```bash
ssh root@stapp01
# expected: "Permission denied" or "Connection closed"
```
3. Confirm config contains the change:
```bash
sudo grep -i PermitRootLogin /etc/ssh/sshd_config
# expected output: PermitRootLogin no
```

Explanation:
- `sudo -v` verifies the sudo credential on the non-root account.
- `grep -i` checks the `sshd_config` for `PermitRootLogin`. Use `-i` for case-insensitive search.

---

## Rollback / Recovery (if you get locked out)
If you lose access:
- If you have console access (VM host, cloud provider serial/console, or KVM), use it to log in and revert the change in `/etc/ssh/sshd_config`.
- If you have a separate management network or jump host with access, use that.
- If neither console nor management access exists, contact your datacenter/cloud provider for rescue/recovery.

To revert:
```bash
# edit file via console
sudo nano /etc/ssh/sshd_config
# change back e.g. PermitRootLogin yes
sudo systemctl restart sshd
```

---

## Additional SSH Hardening Tips (expanded)
1. Use key-based authentication (disable passwords)
   - Generate keys on client and copy public key to `~/.ssh/authorized_keys` of the user.
   - In `sshd_config`:
     ```
     PasswordAuthentication no
     PubkeyAuthentication yes
     ```
   - Benefit: removes password guessing attacks.

2. Disable root login (already covered)
   - `PermitRootLogin no`

3. Restrict which users may SSH in
   - Use `AllowUsers` or `AllowGroups`:
     ```
     AllowUsers thor steve banner
     # or
     AllowGroups sshusers
     ```
   - Benefit: only listed accounts can authenticate via SSH.

4. Use SSH key options for additional constraints
   - In `authorized_keys` prepend options like:
     ```
     no-port-forwarding,no-agent-forwarding,no-X11-forwarding,command="/usr/local/bin/restrict-shell" ssh-ed25519 AAAA...
     ```
   - Benefit: restrict what a key can do even if compromised.

5. Use modern key types and rotate keys
   - Prefer `ed25519` or `rsa` with >=3072 bits.
   - Periodically rotate authorized keys and remove stale keys.

6. Enable two-factor authentication (2FA) for SSH
   - Use `Google Authenticator`, `Duo`, or `privacyIDEA`.
   - Example: configure `pam_oath` or Duo PAM module to require OTP + key.

7. Limit authentication attempts and timeouts
   - In `sshd_config`:
     ```
     LoginGraceTime 30s
     MaxAuthTries 3
     MaxSessions 2
     ```
   - Benefit: reduces brute-force window.

8. Restrict access via firewall / network
   - Use firewall rules to allow SSH only from trusted IPs or jump hosts.
   - Example (ufw):
     ```bash
     sudo ufw allow from 203.0.113.0/24 to any port 22
     sudo ufw deny 22
     ```
   - Or use security groups on cloud providers.

9. Use an SSH jump/bastion host
   - Put servers in private network and expose SSH only through a hardened bastion with strict logging and MFA.

10. Run rate-limiting / intrusion prevention
    - Use Fail2Ban, DenyHosts, or firewall rate limits to block repeated failed attempts.
    - Example Fail2Ban install (Debian):
      ```bash
      sudo apt install fail2ban
      sudo systemctl enable --now fail2ban
      ```

11. Use SSH certificates (short-lived)
    - Certificate authority (CA)-signed user keys with short TTLs reduce key management burden.

12. Disable unused features
    - `PermitEmptyPasswords no`
    - `AllowAgentForwarding no`
    - `X11Forwarding no` (if not needed)
    - `TCPKeepAlive yes` (tune per environment)

13. Jail users when needed
    - Use `ChrootDirectory` for restricted accounts that require only limited access.

14. Centralize SSH account & key management
    - Use LDAP, SSSD, or an identity provider to centrally manage user access and revoke privileges quickly.

15. Ensure good logging & monitoring
    - Forward auth logs to a SIEM or centralized logging (rsyslog, syslog-ng)
    - Monitor for suspicious authentication attempts and key additions.

16. Enforce configuration compliance
    - Use automation (Ansible, Chef, Puppet) to enforce `sshd_config` across servers and prevent drift.

---

## Why This Matters (expanded)
- Reduces attack surface: Disabling root SSH eliminates a widely-known privileged account that attackers target with brute-force attempts.
- Enforces least privilege: Users must log in with an individual account and escalate to root via `sudo`, improving separation of duties.
- Improved auditing & accountability: Actions are tied to user accounts (with sudo logging) rather than anonymous root activity.
- Limits credential exposure: If a non-root user's key or password is compromised, it is easier to revoke that single account than shared root credentials.
- Supports incident response & forensics: Individual accounts make it clearer who performed which actions, simplifying investigations.
- Compliance & policy alignment: Many standards (PCI-DSS, CIS Benchmarks, corporate security policies) require disabling direct root SSH login or enforcing accountable admin access.
- Reduced blast radius: With root login disabled and tightened access controls (key restrictions, allowlists), a compromised account or key is less likely to lead to full system compromise.
- Encourages operational best practices: Use of bastions, 2FA, centralized identity management, and short-lived credentials become natural follow-ups.

---

## Example: Full change + verification for one server
```bash
# 1. log in as a sudo user
ssh thor@stapp01

# 2. make the change interactively (recommended)
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
# ensure line: PermitRootLogin no

# 3. test in a separate terminal: confirm you can ssh and sudo
# (open new terminal and run:)
ssh thor@stapp01
sudo -v

# 4. restart sshd (after verification)
sudo systemctl restart sshd

# 5. verify
sudo grep -i PermitRootLogin /etc/ssh/sshd_config
ssh root@stapp01   # should be denied
```

---

## Notes & Best Practices
- Always keep an administrative out-of-band or console method available (cloud serial console, remote KVM, etc.) in case network SSH access is lost.
- Make changes using automation where possible (Ansible playbook, etc.) to ensure consistent, auditable changes across servers.
- Test changes on one server before rolling out to all production servers.

---

References
- OpenSSH manual: `man sshd_config`
- CIS Benchmarks for Linux SSH configuration
