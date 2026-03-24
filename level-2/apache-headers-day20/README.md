# 🔐 Apache Security Headers Configuration (KodeKloud Lab)

## 📌 Overview

In this lab, we configured an Apache web server with security hardening by enabling important HTTP security headers. This helps protect web applications from common attacks like XSS, clickjacking, and MIME sniffing.

---

## 🧠 Problem Statement

- Install Apache (httpd) on App Server 3
- Configure Apache to run on port `6100`
- Create a static web page under `/var/www/html`
- Enable the following security headers:
  - X-XSS-Protection
  - X-Frame-Options
  - X-Content-Type-Options

---

## 🛠️ Implementation Steps

### 1️⃣ Install Apache

```bash
sudo yum install -y httpd
````

---

### 2️⃣ Configure Apache Port

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Update:

```apache
Listen 6100
```

---

### 3️⃣ Start and Enable Apache

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

### 4️⃣ Create Web Page

```bash
cd /var/www/html
sudo vi index.html
```

Add content:

```html
Welcome to the xFusionCorp Industries!
```

---

### 5️⃣ Enable mod_headers Module

Check if module is loaded:

```bash
httpd -M | grep headers
```

If not present, edit:

```bash
sudo vi /etc/httpd/conf.modules.d/00-base.conf
```

Ensure this line exists and is uncommented:

```apache
LoadModule headers_module modules/mod_headers.so
```

---

### 6️⃣ Configure Security Headers

Edit main config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Add at the end:

```apache
<IfModule mod_headers.c>
    Header set X-XSS-Protection "1; mode=block"
    Header set X-Frame-Options "SAMEORIGIN"
    Header set X-Content-Type-Options "nosniff"
</IfModule>
```

---

### 7️⃣ Restart Apache

```bash
sudo systemctl restart httpd
```

---

## ✅ Verification

```bash
curl -I http://localhost:6100
```

Expected output:

```
X-XSS-Protection: 1; mode=block
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
```

---

## 🎯 Key Learnings

* Apache configuration management
* Enabling HTTP security headers
* Basic web server hardening
* Verifying headers using curl

---

## 🔐 Security Headers Explained

| Header                 | Purpose                             |
| ---------------------- | ----------------------------------- |
| X-XSS-Protection       | Prevents cross-site scripting (XSS) |
| X-Frame-Options        | Prevents clickjacking attacks       |
| X-Content-Type-Options | Prevents MIME-type sniffing         |

---

## 💼 Resume Point

> Configured Apache web server with security headers to enhance application security and mitigate common web vulnerabilities.

---

## 👨‍💻 Author

**Pankaj Sharma**

