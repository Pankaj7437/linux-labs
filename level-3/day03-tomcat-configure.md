# 🚀 Tomcat Deployment Lab (Root Cause + Complete Detailed Guide)

---

# 📌 Objective

Deploy a Java application on **Tomcat (App Server 3)** such that:

* Tomcat installed ✔
* Running on **port 6100** ✔
* `ROOT.war` deployed ✔
* Accessible via:

```
http://stapp03:6100
```

---

# ⚠️ IMPORTANT ISSUE YOU FACED

👉 Tomcat start ho raha tha but turant stop ho raha tha:

```
Active: inactive (dead)
```

---

## 🔍 Root Cause

In your config:

```xml
<Server port="8080" shutdown="SHUTDOWN">
```

❌ Problem:

* `8080` already commonly used hota hai
* Ye HTTP port nahi hai → **shutdown port hai**
* Conflict hone par Tomcat immediately stop ho jata hai

---

## ✅ Fix

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

✔ Default correct value
✔ Internal shutdown communication ke liye

---

# 🖥️ Step-by-Step Implementation

---

## 🔹 Step 1: Copy WAR file

From jump host:

```bash
scp /tmp/ROOT.war banner@stapp03:/tmp/
```

### 🧠 Explanation:

* WAR file = Java web application package
* SCP = secure copy to remote server

---

## 🔹 Step 2: Login to App Server

```bash
ssh banner@stapp03
```

---

## 🔹 Step 3: Install Tomcat

```bash
sudo yum install tomcat -y
```

### 📍 Important paths:

| Path                       | Purpose      |
| -------------------------- | ------------ |
| `/etc/tomcat/`             | config files |
| `/var/lib/tomcat/webapps/` | deployment   |
| `/var/log/tomcat/`         | logs         |

---

## 🔹 Step 4: Configure Port (6100)

```bash
sudo vi /etc/tomcat/server.xml
```

Find:

```xml
<Connector port="8080"
```

Replace:

```xml
<Connector port="6100" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

## 🧠 Explanation:

| Attribute         | Meaning             |
| ----------------- | ------------------- |
| port              | HTTP listening port |
| protocol          | HTTP handling       |
| connectionTimeout | request timeout     |

---

## 🔹 Step 5: Fix Shutdown Port (CRITICAL)

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

---

## 🧠 Why Important?

* Ye internal control port hai
* Agar conflict hua → Tomcat shutdown

---

## 🔹 Step 6: Deploy Application

```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
```

---

## 🧠 Why ROOT.war?

👉 Special behavior:

| File     | URL            |
| -------- | -------------- |
| ROOT.war | `/` (base URL) |
| app.war  | `/app`         |

So:

```
http://stapp03:6100
```

instead of:

```
http://stapp03:6100/app
```

---

## 🔹 Step 7: Set Permissions

```bash
sudo chown tomcat:tomcat /var/lib/tomcat/webapps/ROOT.war
```

---

## 🔹 Step 8: Start Tomcat

```bash
sudo systemctl restart tomcat
sudo systemctl enable tomcat
```

---

## 🔹 Step 9: Verify Service

```bash
sudo systemctl status tomcat
```

Expected:

```
Active: active (running)
```

---

## 🔹 Step 10: Test Application

```bash
curl http://stapp03:6100
```

---

# 🔍 Debugging Section (VERY IMPORTANT 🔥)

---

## 1️⃣ Service not running

```bash
sudo systemctl status tomcat
```

---

## 2️⃣ Check logs

```bash
sudo journalctl -xeu tomcat
```

---

## 3️⃣ Check deployment

```bash
ls /var/lib/tomcat/webapps/
```

Should show:

```
ROOT  ROOT.war
```

---

## 4️⃣ Port check

```bash
sudo ss -tulnp | grep 6100
```

---

## 5️⃣ Direct debug run

```bash
sudo -u tomcat /usr/libexec/tomcat/server start
```

---

# 🧠 Concepts (Interview Level)

---

## 🔹 What is Tomcat?

👉 Apache Tomcat = Java Web Server

* Runs Servlets & JSP
* Handles HTTP requests
* Used in Spring Boot apps

---

## 🔹 What is WAR file?

👉 Web Application Archive

Contains:

* HTML / JSP
* Java classes
* config files

---

## 🔹 Tomcat Architecture

```
Server
 └── Service
      ├── Connector (port)
      └── Engine
           └── Host
                └── Webapps
```

---

## 🔹 Important Ports

| Port | Purpose     |
| ---- | ----------- |
| 6100 | HTTP access |
| 8005 | shutdown    |
| 8443 | SSL         |

---

# ⚡ Final Workflow

```
Install → Configure → Fix shutdown port → Deploy → Start → Test
```

---

# ✅ Final Result

✔ Tomcat running
✔ Port 6100 configured
✔ Application deployed
✔ Accessible via browser

---

# 🎯 Conclusion

👉 Tera main issue config nahi tha
👉 Sirf **shutdown port conflict** tha

---

🔥 Lab Successfully Completed
