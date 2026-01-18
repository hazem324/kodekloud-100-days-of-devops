# SElinux Installation and Configuration

## Objective

Following a security audit, the xFusionCorp Industries security team decided to introduce SELinux to improve server and application security.

This change applies ONLY to **App Server 3** in the Stratos Datacenter for initial testing.  
SELinux must be installed but permanently **disabled** so it remains disabled after the next reboot. SELinux will be re-enabled later after required policy tuning.

> ⚠️ The current runtime SELinux status reported by tools can be ignored. The expected final state after reboot is **disabled**.

---

## Requirements

- Install the required SELinux packages.
- Permanently disable SELinux (persist across reboots).
- Do NOT reboot the server now (maintenance reboot already scheduled).
- Ensure SELinux remains disabled after the scheduled reboot.

---

## 1. Install SELinux Packages

Install required packages with `dnf` (or `yum` on older systems):

```bash
sudo dnf install -y \
  selinux-policy \
  selinux-policy-targeted \
  policycoreutils \
  policycoreutils-python-utils
```

What these provide:
- SELinux policy packages
- Management utilities (policycoreutils)
- Policy tooling for future configuration

(If your distro uses `yum`, replace `dnf` with `yum`.)

---

## 2. Edit the Persistent SELinux Config

Open the persistent config:

```bash
sudo nano /etc/selinux/config
```

Find the `SELINUX=` line and set it exactly to:

```
SELINUX=disabled
```

Important:
- Use `disabled` (not `disabed`). No typos.
- Ensure the line is not commented (no leading `#`).
- Save and exit.

Note: `setenforce` affects runtime only and does not persist across reboots. Editing `/etc/selinux/config` controls boot-time behavior.

---

## 3. Verify the Configuration Change

Run:

```bash
grep SELINUX= /etc/selinux/config
```

Expected output:

```
SELINUX=disabled
```

If `getenforce` or `sestatus` still shows a different runtime mode now, that is fine — the file controls the state applied after the scheduled reboot.

---

## 🧠 Good to Know 

### What is SELinux?
SELinux (Security-Enhanced Linux) is a kernel-level Mandatory Access Control (MAC) implementation that can restrict actions beyond traditional Unix permissions (DAC), including restricting `root` if the policy disallows an action.

### SELinux Modes

| Mode       | Effect |
|------------|--------|
| Enforcing  | SELinux enforces policy; violations are blocked |
| Permissive | Violations are allowed but logged |
| Disabled   | SELinux is turned off |

Reminder: This task requires SELinux to be **disabled** after reboot, not set to `permissive`.

### Why install but disable?
- Packages must be present before creating/applying policies.
- Enabling SELinux too early may break applications.
- Teams can develop and test policies without causing outages.

---

## Key Files

| Path | Purpose |
|------|---------|
| /etc/selinux/config | Persistent SELinux mode (boot-time) |
| /var/log/audit/audit.log | SELinux denial and audit logs |
| /etc/selinux/targeted/ | Default targeted policy files |

---

## Useful Commands (reference)
- getenforce — show current runtime mode
- sestatus — detailed SELinux status
- setenforce 0 — set runtime to permissive (temporary)
- setenforce 1 — set runtime to enforcing (temporary if not disabled in config)