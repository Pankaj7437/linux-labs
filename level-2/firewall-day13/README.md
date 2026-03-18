# Firewalld + Nginx Reverse Proxy Setup – App Server 3

## Objective

Configure firewall and services on **App Server 3 (stapp03)**:

- Allow port 80 (Nginx)
- Block port 8085 (Apache)
- Ensure rules are permanent
- Use public zone
- Ensure services are running

---

# Problem Faced

Nginx failed to start due to:

```
bind() to 0.0.0.0:80 failed (Address already in use)
```

### Root Cause

Apache (httpd) was already running on port 80, causing conflict.

---

# Solution Overview

| Service | Port |
|---|---|
| Nginx | 80 |
| Apache | 8085 |

---

# Step 1 – Connect to Server

```bash
ssh banner@stapp03
```

---

# Step 2 – Change Apache Port

Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Update:

```
Listen 8085
```

---

# Step 3 – Restart Apache

```bash
sudo systemctl restart httpd
```

---

# Step 4 – Start Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

# Step 5 – Install and Enable firewalld

```bash
sudo yum install firewalld -y
sudo systemctl enable firewalld --now
```

---

# Step 6 – Configure Firewall

Allow port 80:

```bash
sudo firewall-cmd --permanent --add-port=80/tcp
```

Block port 8085:

```bash
sudo firewall-cmd --permanent --remove-port=8085/tcp
```

Reload:

```bash
sudo firewall-cmd --reload
```

---

# Step 7 – Verify Ports

```bash
sudo ss -tulpn | grep -E '80|8085'
```

---

# Step 8 – Verify Firewall

```bash
firewall-cmd --list-ports
```

Expected:

```
80/tcp
```

---

# Commands Cheat Sheet

```bash
sudo vi /etc/httpd/conf/httpd.conf

sudo systemctl restart httpd

sudo systemctl start nginx

sudo yum install firewalld -y

sudo systemctl enable firewalld --now

sudo firewall-cmd --permanent --add-port=80/tcp

sudo firewall-cmd --permanent --remove-port=8085/tcp

sudo firewall-cmd --reload
```

---

# Common Mistakes

| Mistake | Issue |
|---|---|
| Apache on port 80 | Nginx fails to start |
| Not restarting httpd | Port change not applied |
| Not reloading firewall | Rules not applied |
| Using same port for both | Conflict |

---

# Key Learning

- Port conflict troubleshooting
- Reverse proxy architecture
- Firewalld configuration
- Service dependency handling

---

# Conclusion

Successfully configured **Nginx on port 80** and **Apache on port 8085**, and secured the server using firewalld with proper rules.
