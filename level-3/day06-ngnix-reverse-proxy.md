# 🚀 KodeKloud Task: Configure Apache (httpd) with Nginx Reverse Proxy

## 📌 Objective

Configure **Apache (httpd)** and **Nginx** on the **Nautilus Backup Server** so that:

* Apache serves the website on **port 5003**
* Nginx listens on **port 8098**
* Nginx works as a **Reverse Proxy** for Apache
* A sample `index.html` from the **Jump Host** is copied to Apache's DocumentRoot
* Both services start successfully

---

# Step 1: SSH into the Backup Server

```bash
ssh clint@<backup-server>
```

---

# Step 2: Install Apache (httpd)

```bash
sudo yum install -y httpd
```

Verify installation:

```bash
httpd -v
```

---

# Step 3: Configure Apache to Listen on Port 5003

Edit Apache configuration:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Find:

```apache
Listen 80
```

Replace with:

```apache
Listen 5003
```

> **Important:** Do **not** bind Apache to `127.0.0.1:5003`. Keep it as:

```apache
Listen 5003
```

This allows Apache to listen on all network interfaces.

---

# Step 4: Install Nginx

```bash
sudo yum install -y nginx
```

Verify installation:

```bash
nginx -v
```

---

# Step 5: Configure Nginx

Edit configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Inside the `server` block change:

```nginx
listen 80;
```

to

```nginx
listen 8098;
```

Configure reverse proxy:

```nginx
location / {
    proxy_pass http://127.0.0.1:5003;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

---

# Step 6: Copy the Website from Jump Host

The sample webpage exists on the **Jump Host**, not on the Backup Server.

From the **Backup Server**, copy the file:

```bash
scp thor@jump_host:/home/thor/index.html /tmp/
```

Copy it into Apache's DocumentRoot:

```bash
sudo cp /tmp/index.html /var/www/html/
```

Verify:

```bash
ls /var/www/html
```

---

# Step 7: Test Configurations

Apache:

```bash
sudo apachectl configtest
```

Expected:

```text
Syntax OK
```

Nginx:

```bash
sudo nginx -t
```

Expected:

```text
syntax is ok
test is successful
```

---

# Step 8: Start and Enable Services

Apache:

```bash
sudo systemctl enable --now httpd
```

Nginx:

```bash
sudo systemctl enable --now nginx
```

Verify:

```bash
systemctl status httpd
systemctl status nginx
```

---

# Step 9: Test the Reverse Proxy

Use curl:

```bash
curl http://<backup-server-ip>:8098
```

or

```bash
curl http://localhost:8098
```

The contents of the copied `index.html` should be displayed.

---

# Commands Used

```bash
sudo yum install -y httpd
sudo yum install -y nginx

sudo vi /etc/httpd/conf/httpd.conf
sudo vi /etc/nginx/nginx.conf

scp thor@jump_host:/home/thor/index.html /tmp/
sudo cp /tmp/index.html /var/www/html/

sudo apachectl configtest
sudo nginx -t

sudo systemctl enable --now httpd
sudo systemctl enable --now nginx

curl http://localhost:8098
```

---

# Key Learnings

* Installed and configured **Apache (httpd)** as the backend web server.
* Changed Apache's listening port from **80** to **5003**.
* Installed and configured **Nginx** to listen on **8098**.
* Implemented **Nginx Reverse Proxy** using the `proxy_pass` directive.
* Copied website content securely from a remote server using **SCP**.
* Validated Apache and Nginx configurations before restarting services.
* Managed Linux services with **systemctl**.
* Verified end-to-end communication using **curl**, confirming that Nginx successfully forwarded client requests to Apache.

> **DevOps Takeaway:** Reverse proxies are commonly used in production to improve scalability, security, SSL termination, load balancing, and request routing. In this task, Nginx acted as the frontend proxy while Apache (httpd) served the application behind it.
