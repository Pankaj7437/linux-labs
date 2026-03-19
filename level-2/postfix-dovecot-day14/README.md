# 📧 DevOps Lab: Setup Mail Server using Postfix & Dovecot

## 📌 Objective

- Install and configure Postfix (SMTP)
- Create a mail user
- Configure Maildir
- Install and configure Dovecot (IMAP/POP3)
- Verify services

---

## 🧠 Architecture

User → Postfix (SMTP) → Maildir → Dovecot (IMAP/POP3) → User

---

## 🛠️ Step 1: Install Packages

sudo yum install postfix dovecot -y

---

## ⚙️ Step 2: Configure Postfix

sudo vi /etc/postfix/main.cf

Add/Update:

myhostname = stmail01.stratos.xfusioncorp.com  
mydomain = stratos.xfusioncorp.com  
myorigin = $mydomain  
inet_interfaces = all  
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain  
home_mailbox = Maildir/

Start service:

sudo systemctl enable postfix  
sudo systemctl start postfix  

---

## 👤 Step 3: Create User

sudo useradd ammar  
sudo passwd ammar  

---

## 📂 Step 4: Setup Maildir

sudo su - ammar  
mkdir -p Maildir/{cur,new,tmp}  
exit  

---

## ⚙️ Step 5: Configure Dovecot

sudo vi /etc/dovecot/dovecot.conf  

Add:

protocols = imap pop3  

---

sudo vi /etc/dovecot/conf.d/10-mail.conf  

Update:

mail_location = maildir:~/Maildir  

---

## ▶️ Step 6: Start Dovecot

sudo systemctl enable dovecot  
sudo systemctl start dovecot  

---

## 🔍 Step 7: Verify Services

sudo systemctl status postfix  
sudo systemctl status dovecot  

---

## 🌐 Step 8: Check Ports

ss -tulnp  

| Port | Service |
|------|--------|
| 25   | SMTP |
| 110  | POP3 |
| 143  | IMAP |

---

## 🧪 Step 9: Testing

SMTP Test:

telnet localhost 25  

Expected:  
220 stmail01.stratos.xfusioncorp.com ESMTP Postfix  

---

POP3 Test:

telnet localhost 110  

Expected:  
+OK Dovecot ready.  

---

## ⚠️ Common Mistakes

- Maildir structure missing (cur, new, tmp)  
- home_mailbox not set  
- Wrong mail_location  
- Services not started  

---

## ✅ Result

- Postfix running  
- Dovecot running  
- Maildir configured  
- Mail services verified  

---

## 🚀 Author

Pankaj Sharma
