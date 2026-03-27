# 📘 Squid Log Rotation Configuration (KodeKloud Lab)

## 🧾 Task Summary

Configure log rotation for **Squid proxy logs** on all app servers:

* Install required packages
* Set log rotation to **monthly**
* Keep only **3 rotated logs**

---

## ⚙️ Steps Performed

### 1️⃣ Install Squid (if not already installed)

```bash
sudo yum install -y squid
```

---

### 2️⃣ Install Logrotate (if not already installed)

```bash
sudo yum install -y logrotate
```

---

### 3️⃣ Edit Squid Logrotate Configuration

```bash
sudo vi /etc/logrotate.d/squid
```

---

### 4️⃣ Update Configuration

#### 🔧 Changes Required:

* Change rotation frequency to **monthly**
* Keep only **3 logs**

#### ✅ Final Configuration:

```conf
/var/log/squid/*.log {
    monthly
    rotate 3
    compress
    delaycompress
    notifempty
    missingok
    sharedscripts
    postrotate
        /usr/sbin/squid -k rotate 2>/dev/null
    endscript
}
```

---

### 5️⃣ Save and Exit

```
:wq
```

---

## 🚨 Important Notes

* Do **NOT** start or enable `logrotate` service manually
* Logrotate runs automatically via system timer/cron
* Only configuration change is required for this task

---

## ✅ Verification

Check configuration:

```bash
cat /etc/logrotate.d/squid
```

---

## 🎯 Result

* Squid logs will rotate **monthly**
* Only **3 rotated logs** will be retained
* Logs will be compressed automatically

---

## 👨‍💻 Author

Pankaj Sharma
