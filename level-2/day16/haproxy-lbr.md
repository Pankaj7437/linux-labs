# 🚀 Configure HAProxy Load Balancer for Apache Servers

## 📌 Overview
This project demonstrates how to configure **HAProxy as a Load Balancer (LBR)** for a static website running on multiple application servers.

All backend servers already have Apache running on port **5004**, and HAProxy is configured to distribute incoming traffic across them.

---

## 🏗️ Architecture
- **Load Balancer:** HAProxy (LBR server)
- **Backend Servers:**
  - stapp01:5004
  - stapp02:5004
  - stapp03:5004
- **Protocol:** HTTP
- **Frontend Port:** 80

---

## ⚠️ Requirements
- Install HAProxy using `yum`
- Configure load balancing for all app servers
- Use existing HAProxy configuration (do not create new frontend/backend)
- Do **NOT remove** the `stats socket` entry
- Ensure service works on `http://localhost:80`

---

## 🧰 Implementation Steps

### 1. Install HAProxy
```bash
sudo yum install -y haproxy
````

---

### 2. Configure HAProxy

```bash
sudo vi /etc/haproxy/haproxy.cfg
```

Modify existing configuration:

#### 🔹 Update Frontend

```haproxy
frontend main
    bind *:80
    default_backend app
```

#### 🔹 Update Backend

```haproxy
backend app
    balance roundrobin
    server app1 stapp01:5004 check
    server app2 stapp02:5004 check
    server app3 stapp03:5004 check
```

---

### 3. Important Note

Do **NOT remove** the following line from configuration:

```haproxy
stats socket /var/lib/haproxy/stats
```

---

### 4. Restart HAProxy

```bash
sudo systemctl restart haproxy
sudo systemctl enable haproxy
```

---

## 🔍 Verification

```bash
curl http://localhost:80
```

If configured correctly, the request will be served from one of the backend servers.

---

## ⚡ Load Balancing Method

* **Round Robin** algorithm is used
* Requests are distributed evenly across all backend servers

---

## 🐞 Troubleshooting

### HAProxy not starting

```bash
haproxy -c -f /etc/haproxy/haproxy.cfg
```

### Service not responding

```bash
systemctl status haproxy
```

### Backend issue

```bash
curl stapp01:5004
```

---

## 🧠 Key Learnings

* HAProxy configuration basics
* Load balancing using round robin
* Modifying existing configs instead of creating new ones
* Importance of following exact instructions

---

## 🏁 Conclusion

Successfully configured HAProxy as a load balancer to distribute traffic across multiple Apache servers, ensuring high availability and efficient request handling.

---

## 🏷️ Tags

haproxy, load-balancer, devops, linux, networking, kodekloud



## 👨‍💻 Author
**Pankaj Sharma**  
B.Tech CSE Student | DevOps Learner  
