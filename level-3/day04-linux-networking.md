# Apache Port 5003 Troubleshooting – Stratos Datacenter

## Objective
Fix the issue where the Apache service on **App Server 1 (stapp01)** was not reachable on **port 5003** from the jump host.

Target URL:
http://stapp01:5003

---

## Problem
Apache service failed to start because **port 5003 was already in use by another service**.  
The following error was observed:

(98) Address already in use  
make_sock: could not bind to address

Additionally, the server was using **iptables firewall**, which contained a **REJECT rule**.  
Even after allowing port 5003, traffic was still blocked because firewall rules are evaluated **from top to bottom**.
It has been the most time taking task for me , it took me 4-5 redos but overall worth it.
---

## Steps Performed

### 1. Login to App Server 1
ssh tony@stapp01

### 2. Check Apache Service Status
sudo systemctl status httpd

### 3. Identify Which Process Was Using Port 5003
sudo ss -tulpn | grep 5003

### 4. Stop the Conflicting Service
sudo systemctl stop <service-name>
or kill the process using process id (pid) 

### 5. Start Apache Service
sudo systemctl start httpd  
sudo systemctl enable httpd

### 6. Verify Apache Listening Port
sudo ss -tulpn | grep httpd

Expected output should show Apache listening on port **5003**.

### 7. Check Firewall Status
sudo systemctl status iptables

### 8. Allow Port 5003 in Firewall
sudo iptables -I INPUT -p tcp --dport 5003 -j ACCEPT

### 9. Move Rule to Top (Important)
To ensure the allow rule is evaluated before the reject rule:

sudo iptables -I INPUT 1 -p tcp --dport 5003 -j ACCEPT

### 10. Save Firewall Rules
sudo iptables-save | sudo tee /etc/sysconfig/iptables

---

## Verification

From the jump host run:

curl http://stapp01:5003

Successful HTML response confirms the Apache service is reachable.

---

## Result

- Apache service running successfully
- Port **5003** open and listening
- Firewall rule added correctly
- Service reachable from jump host
