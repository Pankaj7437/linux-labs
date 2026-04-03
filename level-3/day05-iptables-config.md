# Linux Firewall Configuration – Restrict Port 6100 Using iptables

## Objective
During a security audit in the Nautilus infrastructure, it was discovered that **port 6100** on the application servers was open to all hosts. This creates a security risk.  

The goal of this task is to configure **iptables firewall rules** so that:

- Only the **LBR (Load Balancer) host** can access **port 6100**
- All other incoming traffic to **port 6100** must be blocked
- Firewall rules must **persist after system reboot**

---

# Environment Details

| Parameter | Value |
|----------|------|
| Infrastructure | Nautilus |
| Server Type | Application Servers |
| Firewall Tool | iptables |
| Protected Port | 6100 |
| Allowed Host | LBR (stlb01) |
| Requirement | Firewall rules must persist after reboot |

---

# Initial Situation

Before configuration, the application servers had **no firewall protection for port 6100**.  
This meant that **any host could access the service running on that port**, which is a potential security vulnerability.

---

# Step 1 – Connect to Application Servers

Login to each application server from the jump host.

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

These servers host the application services that need firewall protection.

---

# Step 2 – Install iptables Firewall

Install the iptables package.

```bash
sudo yum install iptables -y
```

Explanation:

| Command | Purpose |
|-------|---------|
| yum install | Installs software packages |
| iptables | Linux firewall tool |
| -y | Automatically confirm installation |

---

# Step 3 – Start and Enable Firewall Service

Start the firewall service.

```bash
sudo systemctl start iptables
```

Enable it so it starts automatically on system boot.

```bash
sudo systemctl enable iptables
```

Check service status.

```bash
sudo systemctl status iptables
```

Explanation:

| Command | Purpose |
|-------|---------|
| systemctl start | Starts the service immediately |
| systemctl enable | Enables the service on boot |
| systemctl status | Shows service status |

---

# Step 4 – Identify the LBR Host IP

To allow only the **Load Balancer (stlb01)**, we first need its IP address.

```bash
getent hosts stlb01
```

Example output:

```
10.244.195.217 stlb01.o4k5z4wmh4gpikio.svc.cluster.local
```

So the **LBR IP address is:**

```
10.244.195.217
```

Explanation:

| Command | Purpose |
|-------|---------|
| getent | Queries system databases |
| hosts | Searches hostname database |
| stlb01 | Load balancer hostname |

---

# Step 5 – Understand Existing Firewall Rules

Check current rules:

```bash
sudo iptables -L
```

Typical output may look like:

```
ACCEPT related,established
ACCEPT icmp
ACCEPT loopback
ACCEPT ssh
REJECT all
```

Important observation:

```
REJECT all
```

This rule **blocks all unmatched traffic**.

Therefore **any new rules added after this rule will not be processed**.

---

# Step 6 – Add Firewall Rules (Correct Order)

The firewall must allow traffic from the **LBR host** before blocking others.

### Allow LBR access to port 6100

```bash
sudo iptables -I INPUT 5 -p tcp -s 10.244.195.217 --dport 6100 -j ACCEPT
```

Explanation:

| Option | Meaning |
|------|--------|
| -I | Insert rule at specific position |
| INPUT | Target chain |
| 5 | Insert at rule position 5 |
| -p tcp | Protocol |
| -s | Source IP |
| --dport 6100 | Destination port |
| -j ACCEPT | Allow traffic |

---

### Block all other access to port 6100

```bash
sudo iptables -I INPUT 6 -p tcp --dport 6100 -j DROP
```

Explanation:

| Option | Meaning |
|------|--------|
| -I INPUT 6 | Insert rule after allow rule |
| -p tcp | TCP protocol |
| --dport 6100 | Target port |
| -j DROP | Silently discard packets |

---

# Resulting Firewall Rule Order

```
ACCEPT related,established
ACCEPT icmp
ACCEPT loopback
ACCEPT ssh
ACCEPT tcp from LBR to port 6100
DROP tcp from others to port 6100
REJECT all
```

This ensures:

- LBR traffic allowed
- All other traffic blocked

---

# Step 7 – Verify Firewall Rules

Check rules with numeric output.

```bash
sudo iptables -L -n
```

Expected result:

```
ACCEPT tcp -- 10.244.195.217 anywhere tcp dpt:6100
DROP   tcp -- anywhere anywhere tcp dpt:6100
```

---

# Step 8 – Save Firewall Rules

Rules must persist after reboot.

```bash
sudo iptables-save | sudo tee /etc/sysconfig/iptables
```

Explanation:

| Command | Purpose |
|-------|---------|
| iptables-save | Export current rules |
| tee | Write output to file |
| /etc/sysconfig/iptables | Persistent firewall config file |

---

# Step 9 – Restart Firewall Service

Restart the firewall service.

```bash
sudo systemctl restart iptables
```

This reloads the saved firewall configuration.

---

# Commands Cheat Sheet

```bash
ssh tony@stapp01

sudo yum install iptables -y

sudo systemctl start iptables
sudo systemctl enable iptables

getent hosts stlb01

sudo iptables -I INPUT 5 -p tcp -s 10.244.195.217 --dport 6100 -j ACCEPT
sudo iptables -I INPUT 6 -p tcp --dport 6100 -j DROP

sudo iptables -L -n

sudo iptables-save | sudo tee /etc/sysconfig/iptables

sudo systemctl restart iptables
```

---

# Common Mistakes

| Mistake | Problem |
|-------|--------|
| Using wrong port | Firewall rule does not match service |
| Using `-A` instead of `-I` | Rule added after REJECT rule |
| Wrong rule order | Traffic blocked before allow rule |
| Not saving rules | Rules lost after reboot |
| Incorrect LBR IP | Legitimate traffic blocked |

---

# Key Concepts Learned

- Linux firewall management
- iptables rule order importance
- Allow specific host access
- Blocking unwanted traffic
- Persistent firewall configuration
- Network security best practices

---

# Conclusion

The firewall on the Nautilus application servers was successfully configured using **iptables** to secure **port 6100**. Only the **LBR host (stlb01)** is allowed to access the port, while all other incoming traffic is blocked. The firewall rules were saved to ensure persistence after system reboot, providing improved security for the application infrastructure.
