# Day 16
## Configure Firewalld to Allow Web Application Port

## Task

The Nautilus system administrators deployed a web UI application on **Nautilus Application Server 3** in **Stratos Datacenter**.

The application runs on **port 8082**, so firewall rules must allow incoming traffic on this port.

### Requirements

1. Install and enable the **firewalld** service.
2. Allow incoming traffic on **port 8082/tcp**.
3. Ensure the firewall **zone is set to public**.

---

# Step 1 – Login to Application Server

First I logged into **App Server 3**.

```bash
ssh banner@stapp03
```

---

# Step 2 – Install firewalld

Since firewalld was not installed, I installed it using yum.

```bash
sudo yum install firewalld
```

This installed several dependencies such as:

- iptables
- nftables
- python3-firewall
- ipset

---

# Step 3 – Check firewalld status

After installation I checked if the service was running.

```bash
systemctl status firewalld
```

Output showed:

```
Active: inactive (dead)
```

So the service was installed but not running.

---

# Step 4 – Start and enable firewalld

To activate firewall protection I started and enabled the service.

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Verification:

```bash
systemctl status firewalld
```

Output:

```
Active: active (running)
```

---

# Step 5 – Exploring systemd configuration (Learning Step)

Since I was not fully sure how firewall rules work internally, I explored systemd service files.

```bash
cd /usr/lib/systemd/system
ls
```

Then I checked the **firewalld service configuration**.

```bash
cat firewalld.service
```

This file showed how firewalld runs as a system service and interacts with DBus.

Example:

```
ExecStart=/usr/sbin/firewalld --nofork --nopid
```

This helped me understand how the firewall daemon starts.

---

# Step 6 – Set firewall zone to public

The requirement stated that the zone must be **public**.

```bash
sudo firewall-cmd --set-default-zone=public
```

Output:

```
Warning: ZONE_ALREADY_SET: public
success
```

This means the zone was already set correctly.

---

# Step 7 – Allow port 8082

Now I added a firewall rule to allow the application port.

```bash
sudo firewall-cmd --permanent --zone=public --add-port=8082/tcp
```

---

# Step 8 – Reload firewall rules

After adding a permanent rule, the firewall must be reloaded.

```bash
sudo firewall-cmd --reload
```

Output:

```
success
```

---

# Step 9 – Verify firewall configuration

To confirm the port is open:

```bash
sudo firewall-cmd --list-ports
```

Output:

```
8082/tcp
```

This confirmed the firewall rule was successfully applied.

---

# Result

The firewall on **Application Server 3** is now properly configured.

✔ firewalld installed  
✔ firewalld service running  
✔ firewall zone set to public  
✔ port **8082/tcp** allowed

The web UI application can now receive incoming traffic.

---

# Key Commands Used

| Command | Purpose |
|------|------|
| yum install firewalld | Install firewall service |
| systemctl start firewalld | Start firewall service |
| systemctl enable firewalld | Enable firewall on boot |
| firewall-cmd --set-default-zone | Set default firewall zone |
| firewall-cmd --add-port | Allow a specific port |
| firewall-cmd --reload | Apply firewall changes |
| firewall-cmd --list-ports | Verify allowed ports |

---

# Learning Outcome

This task helped me understand:

- How **Linux firewall management works**
- How **systemd manages services**
- How to allow application ports using **firewalld**

It also showed how important firewall configuration is for enabling access to web applications running on specific ports.
