# 🚀 Apache Redirect Configuration Lab

## 📌 Objective

Configure Apache on **App Server 3** to:

* Listen on port **3003**
* Add required URL redirects:

  * Non-www → www (**301 Permanent**)
  * `/blog` → `/news` (**302 Temporary**)

---

## 🖥️ Server Details

* Service: Apache (`httpd`)
* Config file: `/etc/httpd/conf/httpd.conf`
* Port: **3003**

---

## ⚙️ Step 1: Configure Apache Port

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Find:

```apache
Listen 80
```

Replace with:

```apache
Listen 3003
```

---

## 🔁 Step 2: Add Redirect Rules

Add the following at the end of the file:

```apache
# Enable rewrite engine
RewriteEngine On

# Redirect non-www to www (Permanent - 301)
RewriteCond %{HTTP_HOST} ^stapp03.stratos.xfusioncorp.com [NC]
RewriteRule ^(.*)$ http://www.stapp03.stratos.xfusioncorp.com:3003/$1 [R=301,L]

# Redirect /blog to /news (Temporary - 302)
Redirect 302 /blog http://www.stapp03.stratos.xfusioncorp.com:3003/news
```

---

## 🧠 Explanation of Configuration

### 🔹 RewriteEngine On

* Enables Apache rewrite module
* Required to use RewriteCond and RewriteRule

### 🔹 RewriteCond %{HTTP_HOST} ^stapp03.stratos.xfusioncorp.com [NC]

* Checks incoming request host
* Matches only **non-www domain**
* `[NC]` → case-insensitive match

### 🔹 RewriteRule ^(.*)$ [http://www.stapp03.stratos.xfusioncorp.com:3003/$1](http://www.stapp03.stratos.xfusioncorp.com:3003/$1) [R=301,L]

* Redirects all requests to **www version**
* `^(.*)$` → matches full URL path
* `$1` → passes same path after redirect
* `[R=301]` → permanent redirect
* `[L]` → stop further processing

### 🔹 Redirect 302 /blog [http://www.stapp03.stratos.xfusioncorp.com:3003/news](http://www.stapp03.stratos.xfusioncorp.com:3003/news)

* Redirects `/blog` → `/news`
* `302` → temporary redirect
* Uses simple Apache redirect (no rewrite needed)

---

## 📦 Step 3: Ensure mod_rewrite is Enabled

```bash
sudo httpd -M | grep rewrite
```

If not installed:

```bash
sudo yum install -y mod_rewrite
```

---

## 🔄 Step 4: Restart Apache

```bash
sudo systemctl restart httpd
```

Check status:

```bash
sudo systemctl status httpd
```

---

## 🧪 Step 5: Testing

### 🔹 Test 301 Redirect (non-www → www)

```bash
curl -I http://stapp03.stratos.xfusioncorp.com:3003
```

Expected:

```
HTTP/1.1 301 Moved Permanently
```

---

### 🔹 Test 302 Redirect (/blog → /news)

```bash
curl -I http://www.stapp03.stratos.xfusioncorp.com:3003/blog
```

Expected:

```
HTTP/1.1 302 Found
```

---

## ⚠️ Important Notes

* Use correct **full domain name**
* Do NOT duplicate config lines
* Always restart Apache after changes
* Ensure port **3003** is active

---

## ✅ Final Outcome

* Apache runs on port **3003**
* Non-www redirects to www (**301**)
* `/blog` redirects to `/news` (**302**)

---

🔥 Lab Completed Successfully

