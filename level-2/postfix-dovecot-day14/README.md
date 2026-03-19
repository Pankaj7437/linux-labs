# 📧 Mail Server Setup using Postfix & Dovecot (DevOps Lab)

![Linux](https://img.shields.io/badge/Platform-Linux-blue)
![Postfix](https://img.shields.io/badge/Postfix-SMTP-green)
![Dovecot](https://img.shields.io/badge/Dovecot-IMAP%2FPOP3-orange)
![Status](https://img.shields.io/badge/Project-Completed-success)

---

## 📌 Overview

This project demonstrates the setup of a **complete mail server stack** using:

- **Postfix** → Mail Transfer Agent (SMTP)
- **Dovecot** → Mail Delivery Agent (IMAP/POP3)
- **Maildir** → Mail storage format

The setup enables sending and receiving emails locally on a Linux server.

---

## 🧠 Architecture

```
User → Postfix (SMTP) → Maildir → Dovecot (IMAP/POP3) → User
```

---

## 🎯 Objectives

- Configure Postfix for sending emails
- Create a mail user account
- Setup Maildir structure for email storage
- Configure Dovecot for receiving emails
- Validate services using network testing tools

---

## 🛠️ Tech Stack

| Component | Role |
|----------|------|
| Linux | Operating System |
| Postfix | SMTP Server |
| Dovecot | IMAP/POP3 Server |
| Maildir | Mail Storage |
| Telnet | Testing Tool |

---

## ⚙️ Installation

```bash
sudo yum install postfix dovecot -y
```

---

## ⚙️ Postfix Configuration

Edit configuration file:

```bash
sudo vi /etc/postfix/main.cf
```

Update:

```
myhostname = stmail01.stratos.xfusioncorp.com
mydomain = stratos.xfusioncorp.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
home_mailbox = Maildir/
```

Start and enable:

```bash
sudo systemctl enable postfix
sudo systemctl start postfix
```

---

## 👤 User Setup

```bash
sudo useradd ammar
sudo passwd ammar
```

---

## 📂 Maildir Setup

```bash
sudo su - ammar
mkdir -p Maildir/{cur,new,tmp}
exit
```

---

## ⚙️ Dovecot Configuration

### Enable protocols

```bash
sudo vi /etc/dovecot/dovecot.conf
```

```
protocols = imap pop3
```

---

### Set mail location

```bash
sudo vi /etc/dovecot/conf.d/10-mail.conf
```

```
mail_location = maildir:~/Maildir
```

---

### Start service

```bash
sudo systemctl enable dovecot
sudo systemctl start dovecot
```

---

## 🔍 Verification

### Check services

```bash
sudo systemctl status postfix
sudo systemctl status dovecot
```

---

### Check listening ports

```bash
ss -tulnp
```

| Port | Protocol | Service |
|------|--------|--------|
| 25   | SMTP   | Postfix |
| 110  | POP3   | Dovecot |
| 143  | IMAP   | Dovecot |

---

## 🧪 Testing

### SMTP Test

```bash
telnet localhost 25
```

Expected:

```
220 stmail01.stratos.xfusioncorp.com ESMTP Postfix
```

---

### POP3 Test

```bash
telnet localhost 110
```

Expected:

```
+OK Dovecot ready.
```

---

## ⚠️ Troubleshooting

| Issue | Cause | Fix |
|------|------|-----|
| Nginx/Apache conflict | Port already in use | Change port |
| Mail not delivered | Maildir missing | Create cur/new/tmp |
| Connection refused | Service not running | Start service |
| No mailbox | home_mailbox missing | Add Maildir/ |

---

## 🔑 Key Learnings

- Multi-service configuration (Postfix + Dovecot)
- Mail protocols (SMTP, POP3, IMAP)
- Linux service management
- Debugging ports and services
- Mail storage using Maildir

---

## 🚀 Real-World Relevance

This setup reflects a **basic production mail server architecture**, useful in:

- Internal communication systems
- DevOps infrastructure setups
- Linux system administration roles

---

## 📚 Future Improvements

- Enable SSL/TLS encryption
- Add SMTP authentication
- Configure remote mail access
- Integrate spam filtering (SpamAssassin)

---

## 👨‍💻 Author

**Pankaj Sharma**  
DevOps Learner 🚀

---

## ⭐ Support

If you found this project useful, consider giving it a ⭐ on GitHub!
