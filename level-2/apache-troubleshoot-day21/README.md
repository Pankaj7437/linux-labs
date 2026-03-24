# 🔧 Apache Service Troubleshooting (KodeKloud Lab)

## 📌 Overview

In this lab, we troubleshooted an Apache service failure on one of the Nautilus application servers. The issue was caused by a syntax error in the Apache configuration file.

---

## 🧠 Problem Statement

- Apache service was not running on one of the application servers
- Ensure Apache runs on port `3000`
- Document root should be `/var/www/html`
- Service must be running on all application servers
- Verify using curl from jump host

---

## 🔍 Root Cause

Apache failed to start due to **syntax errors in configuration file**:

- Incorrect quotes (`"ServerRoot"`)
- Missing or extra semicolons (`;`)
- Invalid configuration formatting

---

## 🛠️ Troubleshooting Steps

### 1️⃣ Check Service Status

```bash
sudo systemctl status httpd
````

👉 Error observed:

```
Syntax error on line XX of /etc/httpd/conf/httpd.conf
```

---

### 2️⃣ Validate Configuration File

```bash
sudo httpd -t
```

👉 Helps identify exact syntax issue

---

### 3️⃣ Fix Configuration Errors

Edit config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Fix common issues:

* ❌ Wrong:

  ```apache
  ServerRoot "/etc/httpd";
  ```

* ✅ Correct:

  ```apache
  ServerRoot "/etc/httpd"
  ```

Also ensure:

```apache
Listen 3000
DocumentRoot "/var/www/html"
```

---

### 4️⃣ Start Apache

```bash
sudo systemctl start httpd
```

---

### 5️⃣ Enable Service

```bash
sudo systemctl enable httpd
```

---

## ✅ Verification

From jump host:

```bash
curl http://stapp01:3000
curl http://stapp02:3000
curl http://stapp03:3000
```

👉 Output should show webpage content

---

## 🎯 Key Learnings

* Apache service troubleshooting
* Configuration file debugging
* Using `httpd -t` for validation
* Importance of syntax correctness

---

## ⚠️ Common Mistakes

* Adding semicolons (`;`) in Apache config (not required)
* Incorrect quotes usage
* Editing wrong config file
* Forgetting to restart service

---

## 💼 Resume Point

> Diagnosed and resolved Apache service failure by identifying and fixing configuration syntax errors, ensuring successful deployment across multiple servers.

---

## 👨‍💻 Author

**Pankaj Sharma**
