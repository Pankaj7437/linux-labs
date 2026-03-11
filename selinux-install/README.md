# Day 18
## SELinux Installation and Permanent Disable

## Task

Following a security audit, the security team decided to test SELinux on **App Server 3** in the **Stratos Datacenter**.

The task required:

1. Install the required SELinux packages.
2. Permanently disable SELinux.
3. No reboot required immediately (scheduled later).

---

# Step 1 – Login to Application Server

```bash
ssh banner@stapp03
```

---

# Step 2 – Install SELinux packages

```bash
sudo yum install -y policycoreutils selinux-policy selinux-policy-targeted
```

These packages provide the SELinux policies and utilities required for managing SELinux.

---

# Step 3 – Edit SELinux configuration

SELinux permanent configuration is stored in:

```
/etc/selinux/config
```

Open the file:

```bash
sudo vi /etc/selinux/config
```

---

# Step 4 – Disable SELinux permanently

Change the following line:

```
SELINUX=enforcing
```

or

```
SELINUX=permissive
```

to

```
SELINUX=disabled
```

---

# Step 5 – Save configuration

Save and exit the file.

```
ESC
:wq
```

---

# Verification

```bash
cat /etc/selinux/config
```

Expected output:

```
SELINUX=disabled
```

Note: The current status shown by `getenforce` may still show **enforcing** or **permissive** until the system is rebooted.

---

# Result

✔ SELinux packages installed  
✔ SELinux permanently disabled in configuration  

After the next system reboot, SELinux will start in **disabled** mode.
