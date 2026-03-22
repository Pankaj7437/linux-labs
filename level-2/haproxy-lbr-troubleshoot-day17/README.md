# 🚀 HAProxy Troubleshooting & Fix (KodeKloud Lab)

## 📌 Problem Statement
The monitoring tool detected an issue with the **HAProxy service** on the **LBR server (stlb01)**.  
We need to troubleshoot and fix the issue so that the application becomes accessible.

---

## 🎯 Objective
- Fix HAProxy configuration
- Ensure HAProxy service is running
- Verify application access using:

```bash
curl http://stlb01:80
````

---

## 🧠 Root Cause Analysis

### 🔍 Observed Issues

1. **HAProxy config issues**

   * `bind` directive missing in frontend
   * backend configuration commented out

2. **Backend connectivity issue**

   * HAProxy was trying to connect to wrong port (`5004`)
   * Actual application was running on **port 8080**

---

## ⚙️ Fix Steps

### 🔹 Step 1: Open HAProxy config

```bash
sudo vi /etc/haproxy/haproxy.cfg
```

---

### 🔹 Step 2: Fix frontend configuration

```haproxy
frontend main
    bind *:80
    default_backend app
```

---

### 🔹 Step 3: Fix backend configuration

```haproxy
backend app
    balance roundrobin
    server app1 stapp01:8080 check
    server app2 stapp02:8080 check
    server app3 stapp03:8080 check
```

---

### 🔹 Step 4: Validate configuration

```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
```

---

### 🔹 Step 5: Restart HAProxy

```bash
sudo systemctl restart haproxy
sudo systemctl enable haproxy
```

---

## 🧪 Verification

### 🔹 Check backend servers

```bash
curl stapp01:8080
curl stapp02:8080
curl stapp03:8080
```

---

### 🔹 Final test

```bash
curl http://stlb01:80
```

✅ Output should return the application HTML page

---

## 💡 Key Learnings

* HAProxy requires:

  * `bind` to accept traffic
  * Proper backend configuration

* Always verify backend services:

  * Running status
  * Correct port

* Common errors:

  * `503 Service Unavailable` → backend issue
  * `Connection refused` → service not running / wrong port

---

## 🧠 Debugging Strategy

1. Check HAProxy status:

```bash
systemctl status haproxy
```

2. Validate config:

```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
```

3. Test backend:

```bash
curl <server>:<port>
```

---

## 👨‍💻 Author

**Pankaj Sharma**
B.Tech CSE Student | DevOps Enthusiast

---


