# 🚀 MariaDB Service Troubleshooting & Fix (KodeKloud Lab)

## 📌 Overview

In this lab, the Nautilus application was unable to connect to the database. Upon investigation, it was found that the **MariaDB service was down** on the database server.

The objective was to **identify the root cause and restore the MariaDB service**.

---

## 🧠 Problem Statement

- Application unable to connect to database
- MariaDB service not running
- Service failing with `exit-code` error

---

## 🔍 Investigation Steps

### 1. Check MariaDB Service Status

```bash
sudo systemctl status mariadb
````

📌 Result:

* Service was **failed**
* Exit status: `status=1/FAILURE`

---

### 2. Analyze Logs

```bash
sudo journalctl -xeu mariadb
```

📌 Observations:

* Database initialization completed successfully
* Service failed during startup phase

👉 This indicated:

> Initialization was fine, but runtime configuration had issues

---

### 3. Verify Data Directory

```bash
ls -l /var/lib/mysql
```

📌 Result:

* Data directory exists
* Files present → DB initialized correctly

---

### 4. Check Main Configuration

```bash
sudo vi /etc/my.cnf
```

📌 Found:

```ini
[client-server]
!includedir /etc/my.cnf.d
```

👉 Meaning:

* Actual configuration is inside `/etc/my.cnf.d/`

---

### 5. Inspect Included Config Files

```bash
ls /etc/my.cnf.d/
```

```bash
sudo vi /etc/my.cnf.d/mariadb-server.cnf
```

---

## ❗ Root Cause

* **Socket path mismatch**
* Server expected socket in:

  ```
  /var/run/mariadb/mysql.sock
  ```
* But configuration pointed to:

  ```
  /var/lib/mysql/mysql.sock
  ```

👉 This mismatch caused MariaDB to crash during startup.

---

## 🛠️ Fix Implementation

### 1. Update Socket Configuration

Edit config file:

```bash
sudo vi /etc/my.cnf.d/mariadb-server.cnf
```

Add/modify:

```ini
[mysqld]
socket=/var/run/mariadb/mysql.sock

[client]
socket=/var/run/mariadb/mysql.sock
```

---

### 2. Create Runtime Directory

```bash
sudo mkdir -p /var/run/mariadb
sudo chown -R mysql:mysql /var/run/mariadb
```

---

### 3. Remove Old Socket File

```bash
sudo rm -f /var/lib/mysql/mysql.sock
```

---

### 4. Fix Permissions

```bash
sudo chown -R mysql:mysql /var/lib/mysql
```

---

### 5. Restart MariaDB

```bash
sudo systemctl restart mariadb
sudo systemctl status mariadb
```

---

## ✅ Verification

Expected output:

```bash
Active: active (running)
```

---

## 🎯 Key Learnings

* MariaDB uses multiple configuration files
* `/etc/my.cnf` may only include other configs
* Always check `/etc/my.cnf.d/`
* Socket path must be consistent across:

  * `[mysqld]`
  * `[client]`
* Initialization success ≠ Service success
* Logs are critical for debugging

---

## 🔥 DevOps Insight

> Don’t memorize paths. Learn how to find and debug.

---

## 👨‍💻 Author

**Pankaj Sharma**

```
