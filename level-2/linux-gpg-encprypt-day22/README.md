# 🔐 GPG Encryption & Decryption Lab (KodeKloud)

## 📌 Task Overview

* Encrypt file:
  `/home/encrypt_me.txt` → `/home/encrypted_me.asc`

* Decrypt file:
  `/home/decrypt_me.asc` → `/home/decrypted_me.txt`

* Use:

  * Keys from `/home/*_key.asc`
  * User: `kodekloud@kodekloud.com`
  * Passphrase: `kodekloud`

---

## 🛠️ Step-by-Step Solution

### 🔹 Step 1: SSH into Storage Server

```bash
ssh natasha@ststor01
```

---

### 🔹 Step 2: Switch to Root User

```bash
sudo -i
```

---

### 🔹 Step 3: Import GPG Keys

```bash
gpg --import /home/*_key.asc
```

✔ Imports both:

* public key
* private key

---

### 🔹 Step 4: Verify Keys

```bash
gpg --list-keys
```

Expected:

```
kodekloud <kodekloud@kodekloud.com>
```

---

### 🔹 Step 5: Fix Permission Issue (Important)

```bash
chmod 777 /home
```

✔ Required because output file is created in `/home`

---

### 🔹 Step 6: Encrypt File

```bash
gpg --output /home/encrypted_me.asc \
    --encrypt \
    --recipient kodekloud@kodekloud.com \
    /home/encrypt_me.txt
```

👉 When prompted:

```
Use this key anyway? (y/N) y
```

---

### 🔹 Step 7: Decrypt File

```bash
gpg --output /home/decrypted_me.txt \
    --decrypt /home/decrypt_me.asc
```

👉 Enter passphrase:

```
kodekloud
```

---

### 🔹 Step 8: Verify Output

```bash
cat /home/decrypted_me.txt
```

Expected Output:

```
Welcome to xFusionCorp Industries. This is KodeKloud System Administration Lab
```

---

## ⚠️ Common Errors & Fixes

### ❌ Permission Denied

✔ Fix:

```bash
chmod 777 /home
```

---

### ❌ Key Not Found (when using sudo)

✔ Reason:

* Root user has separate GPG keyring

✔ Fix:

```bash
sudo -i
gpg --import /home/*_key.asc
```

---

### ❌ "No assurance this key belongs..."

✔ Solution:

```
Type: y
```

---

## 🧠 Key Concepts

* **Public Key** → Encryption
* **Private Key** → Decryption
* **GPG Keyring** → User-specific (root ≠ normal user)
* **Passphrase** → Protects private key

---

## 📦 Final Outcome

✔ Keys imported successfully
✔ File encrypted → `/home/encrypted_me.asc`
✔ File decrypted → `/home/decrypted_me.txt`
✔ Output verified

---

## ✍️ Author

**Pankaj Sharma ** 🚀
DevOps Learner | Linux & Security Enthusiast

---
