# 🔧 DevOps Lab: Debugging Postfix Configuration Issue

## 📌 Problem Statement

Monitoring system reported an issue with the **Postfix mail server** in the Stork DC environment.  
Although the service appeared to be running, it was not considered healthy.

---

## 🧠 Objective

- Identify the root cause of Postfix issue
- Fix configuration errors
- Ensure service runs without warnings

---

## 🔍 Step 1: Check Service Status

```bash
sudo systemctl status postfix
```

### Observation

- Service was **active (running)** ✅
- But logs showed warnings ⚠️

---

## 🔍 Step 2: Analyze Logs Carefully

From the status output:

```
warning: /etc/postfix/main.cf, line 135: overriding earlier entry
```

### Key Insight

This indicates:

- A configuration parameter is defined **multiple times**
- Postfix is overriding earlier values with later ones

---

## 🔍 Step 3: Identify Duplicate Entries

Search for duplicate configuration lines:

```bash
grep inet_interfaces /etc/postfix/main.cf
```

OR check full file:

```bash
sudo vi /etc/postfix/main.cf
```

### Observation

Example:

```
inet_interfaces = all
...
inet_interfaces = all   ❌ duplicate
```

👉 Same parameter defined more than once

---

## 🛠️ Step 4: Fix Configuration

- Open configuration file:

```bash
sudo vi /etc/postfix/main.cf
```

- Remove duplicate entries
- Keep only one correct value

Example:

```
inet_interfaces = all   ✅ (only once)
```

---

## 🔄 Step 5: Restart Service

```bash
sudo systemctl restart postfix
```

---

## 🔍 Step 6: Verify Fix

```bash
sudo systemctl status postfix
```

### Expected Result

- No warning messages
- Service running cleanly

---

## 🔎 Optional Deep Debug

Check logs directly:

```bash
journalctl -u postfix --no-pager | tail -20
```

OR

```bash
journalctl -xe
```

---

## ⚠️ Root Cause

Duplicate configuration entries in:

```
/etc/postfix/main.cf
```

Postfix behavior:

- Accepts last value
- Throws warning
- Causes monitoring alerts

---

## 🔑 Key Learnings

- "Service running" does not mean "service healthy"
- Always check logs for hidden issues
- Duplicate configs can cause unpredictable behavior
- Debugging requires log analysis, not assumptions

---

## 🚀 Debugging Workflow (Reusable)

1. Check service status  
2. Read logs carefully  
3. Identify warnings/errors  
4. Locate config file  
5. Fix issue  
6. Restart service  
7. Verify again  

---

## ✅ Result

- Postfix running successfully  
- No configuration conflicts  
- Monitoring issue resolved  

---

## 👨‍💻 Author

Pankaj Sharma  
DevOps Learner 🚀
