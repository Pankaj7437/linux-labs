# 📘 IPTables Firewall Configuration (KodeKloud Lab)

## 🧾 Task Summary

Configure firewall on backup server:

* Allow incoming traffic on **Nginx port (8098)**
* Block incoming traffic on **Apache port (6300)**
* Make rules **persistent**

---

## ⚙️ Steps Performed

### 1️⃣ Install IPTables

```bash
sudo yum install -y iptables-services
```

---

### 2️⃣ Start and Enable IPTables Service

```bash
sudo systemctl start iptables
sudo systemctl enable iptables
```

---

### 3️⃣ Add Firewall Rules

#### ✅ Allow Nginx Port (8098)

```bash
sudo iptables -A INPUT -p tcp --dport 8098 -j ACCEPT
```

#### ❌ Block Apache Port (6300)

```bash
sudo iptables -A INPUT -p tcp --dport 6300 -j DROP
```

---

### 4️⃣ Save Rules (IMPORTANT ⚠️)

```bash
sudo iptables-save > /etc/sysconfig/iptables
```

---

### 5️⃣ Restart IPTables Service

```bash
sudo systemctl restart iptables
```

---

## 🚨 Common Mistakes (You did this ❌)

* Running `iptables` without `sudo`
* Using `iptables-save` without `sudo`
* Trying to redirect `>` without root → causes **permission denied**

👉 Correct way:

```bash
sudo sh -c "iptables-save > /etc/sysconfig/iptables"
```

---

## ✅ Verification

Check rules:

```bash
sudo iptables -L
```

Expected:

* Port **8098 → ACCEPT**
* Port **6300 → DROP**

---

## 🎯 Result

* Nginx accessible ✅
* Apache blocked ❌
* Rules persistent after reboot 🔒

---

## 👨‍💻 Author

Pankaj Sharma
