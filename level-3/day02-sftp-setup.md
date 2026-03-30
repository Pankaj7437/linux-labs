# 🚀 SFTP User Configuration Lab (Detailed + Explanation)

## 📌 Objective

Configure a secure SFTP setup on **App Server 3** such that:

* A user `javed` is created
* Password authentication is enabled
* User is restricted to **SFTP only (no SSH shell access)**
* User is jailed inside their home directory (secure environment)

---

## 🖥️ Environment Details

* Server: App Server 3 (`stapp03`)
* Service: SSH (`sshd`)
* Config file: `/etc/ssh/sshd_config`
* User: `javed`
* Group: `ftp`

---

# ⚙️ Step 1: Create SFTP User

```bash
sudo useradd -m -G ftp javed
```

### 🔍 Explanation:

* `-m` → Creates home directory `/home/javed`
* `-G ftp` → Adds user to existing `ftp` group
* Group is used for controlled access permissions

---

## 🔐 Set Password

```bash
sudo passwd javed
```

👉 Enter password: `Ksh85Ujjnb`

### 🔍 Explanation:

* Password authentication is required (as per lab)
* This allows login via SFTP using credentials

---

# ⚙️ Step 2: Configure SSH for SFTP Only

```bash
sudo vi /etc/ssh/sshd_config
```

Add the following at the end:

```bash
Match User javed
    ForceCommand internal-sftp
    PasswordAuthentication yes
    ChrootDirectory %h
    PermitTunnel no
    AllowAgentForwarding no
    AllowTcpForwarding no
```

---

# 🧠 Deep Explanation of Each Line

## 🔹 Match User javed

* Applies rules **only to user `javed`**
* Other users remain unaffected

---

## 🔹 ForceCommand internal-sftp

* Forces user to use **SFTP subsystem only**
* Blocks:

  * SSH shell access ❌
  * Bash terminal ❌
* User cannot run commands like `ls`, `cd` via SSH

👉 This is the core line that makes user **SFTP-only**

---

## 🔹 PasswordAuthentication yes

* Enables login using password
* Required since lab explicitly asks for password auth

---

## 🔹 ChrootDirectory %h

* `%h` = user's home directory
* Locks user inside `/home/javed`

👉 Acts like a **jail (sandbox)**
👉 User cannot access:

* `/etc`
* `/var`
* other users' directories

---

## 🔹 PermitTunnel no

* Disables network tunneling
* Prevents advanced SSH misuse

---

## 🔹 AllowAgentForwarding no

* Prevents forwarding SSH credentials
* Improves security

---

## 🔹 AllowTcpForwarding no

* Blocks port forwarding
* Prevents misuse for proxying traffic

---

# ⚙️ Step 3: Fix Permissions (VERY IMPORTANT ⚠️)

```bash
sudo chown root:root /home/javed
sudo chmod 755 /home/javed
```

---

## 🧠 Why this is required?

👉 SSH **will fail** if:

* Home directory is owned by user ❌

👉 Requirement:

* Chroot directory must be owned by **root**

---

## 📁 Create writable directory

```bash
sudo mkdir /home/javed/upload
sudo chown javed:ftp /home/javed/upload
```

---

## 🧠 Why separate folder?

* User cannot write in `/home/javed` (owned by root)
* So we create:
  👉 `/home/javed/upload` → writable area

---

# ⚙️ Step 4: Restart SSH Service

```bash
sudo systemctl restart sshd
```

---

## 🔍 Why restart?

* SSH reads config only at startup
* Changes apply only after restart

---

# 🧪 Step 5: Testing

```bash
sftp javed@stapp03
```

---

## ✅ Expected Behavior

✔ Login successful
✔ Only SFTP interface opens
❌ No shell access

---

## ❌ If user tries SSH:

```bash
ssh javed@stapp03
```

👉 Result:

* No shell access (blocked)

---

# 🧠 Concept Summary

## 🔐 What is SFTP?

* Secure File Transfer Protocol
* Runs over SSH
* Used for file upload/download securely

---

## 🔒 Why restrict to SFTP only?

* Prevents system access
* Allows only file operations
* Common in:

  * Production servers
  * Client file exchange systems

---

## 🏢 Real-World Use Case

* Clients upload files via SFTP
* No server access given
* Secure isolation via chroot

---

# ⚠️ Common Mistakes

❌ Not setting root ownership of home directory
❌ Forgetting to restart sshd
❌ Missing ForceCommand
❌ Wrong permissions (login fails)

---

# 🔄 Workflow Summary

```text
Create User → Configure SSH → Set Permissions → Restart SSH → Test SFTP
```

---

# ✅ Final Outcome

* User `javed` created successfully
* Password authentication enabled
* SSH shell access blocked
* Only SFTP access allowed
* Secure chroot environment implemented

---

🔥 Lab Completed Successfully
