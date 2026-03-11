# Day 17
## Limiting Maximum Processes for a User

## Task

In the **Stratos Datacenter**, Application Server 1 was experiencing **performance degradation** due to excessive processes created by the **nfsuser** user.

To prevent system resource exhaustion, process limits needed to be configured for the user.

### Requirements

Set process limits for user **nfsuser**:

- Soft limit: **1026**
- Hard limit: **2027**

---

# Step 1 – Login to Application Server 1

First I logged into **App Server 1** from the jump host.

```bash
ssh tony@stapp01
```

---

# Step 2 – Check running processes

To understand system activity I checked the processes using:

```bash
top
```

This helped confirm the system was handling multiple processes and limits were required.

---

# Step 3 – Inspect limits configuration file

Linux user process limits are configured in:

```
/etc/security/limits.conf
```

I opened the file to check existing configuration.

```bash
cat /etc/security/limits.conf
```

This file contains definitions for various resource limits such as:

- CPU usage
- memory
- open files
- number of processes

---

# Step 4 – Edit limits configuration

To configure limits for **nfsuser**, I edited the configuration file with root privileges.

```bash
sudo vim /etc/security/limits.conf
```

---

# Step 5 – Add process limits

At the end of the file I added the following lines:

```
nfsuser soft nproc 1026
nfsuser hard nproc 2027
```

Explanation:

| Field | Meaning |
|------|------|
| nfsuser | Username |
| soft | Soft limit (warning threshold) |
| hard | Maximum allowed limit |
| nproc | Number of processes |
| value | Maximum processes allowed |

---

# Step 6 – Save the configuration

In **vim**, I saved the file using:

```
ESC
:wq
```

---

# Step 7 – Verify configuration

To confirm the configuration was applied correctly:

```bash
grep nfsuser /etc/security/limits.conf
```

Expected output:

```
nfsuser soft nproc 1026
nfsuser hard nproc 2027
```

---

# Result

Process limits were successfully configured for **nfsuser**.

✔ Soft process limit set to **1026**  
✔ Hard process limit set to **2027**

This ensures that the user cannot spawn excessive processes that may degrade server performance.

---

# Commands Used

| Command | Purpose |
|------|------|
| top | Monitor running processes |
| cat | View configuration file |
| vim | Edit configuration file |
| grep | Verify configuration |
| sudo | Execute administrative commands |

---

# Learning Outcome

This task demonstrated how Linux **PAM limits** can be used to control system resources and prevent performance degradation caused by uncontrolled process creation.
