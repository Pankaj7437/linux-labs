# Linux Service Management Lab – Install and Enable NSCD on Application Servers

## Objective
The development team released a new application that requires certain backend dependencies to be installed on all application servers in the **Stratos Datacenter**.

The task is to:

- Install the **nscd** package on all application servers
- Ensure the **NSCD service is enabled to start during system boot**

---

# Environment Details

| Parameter | Value |
|----------|------|
| Infrastructure | Nautilus |
| Data Center | Stratos |
| Servers | stapp01, stapp02, stapp03 |
| Package | nscd |
| Service | nscd |
| Requirement | Enable service at boot |

---

# What is NSCD?

NSCD stands for **Name Service Cache Daemon**.

It improves system performance by caching responses from services like:

- DNS lookups
- User authentication
- Group information

Instead of repeatedly querying external sources, the system retrieves cached results from NSCD.

---

# Step 1 – Connect to Application Servers

Login to each server from the jump host.

### App Server 1

```bash
ssh tony@stapp01
```

### App Server 2

```bash
ssh steve@stapp02
```

### App Server 3

```bash
ssh banner@stapp03
```

---

# Step 2 – Install NSCD Package

Install the required package.

```bash
sudo yum install nscd -y
```

Explanation:

| Command | Purpose |
|-------|---------|
| yum install | Installs packages from repository |
| nscd | Name Service Cache Daemon |
| -y | Auto confirm installation |

---

# Step 3 – Start NSCD Service

Start the service immediately.

```bash
sudo systemctl start nscd
```

---

# Step 4 – Enable Service on Boot

Enable NSCD to automatically start after system reboot.

```bash
sudo systemctl enable nscd
```

Explanation:

| Command | Purpose |
|-------|---------|
| systemctl enable | Enable service during boot |
| nscd | Service name |

---

# Step 5 – Verify Service Status

Check if the service is running.

```bash
sudo systemctl status nscd
```

Expected output:

```
Active: active (running)
```

---

# Step 6 – Repeat on All Application Servers

Perform the same installation and service enablement on:

- stapp01
- stapp02
- stapp03

---

# Commands Cheat Sheet

```bash
ssh tony@stapp01
sudo yum install nscd -y
sudo systemctl start nscd
sudo systemctl enable nscd

ssh steve@stapp02
sudo yum install nscd -y
sudo systemctl start nscd
sudo systemctl enable nscd

ssh banner@stapp03
sudo yum install nscd -y
sudo systemctl start nscd
sudo systemctl enable nscd
```

---

# Common Mistakes

| Mistake | Problem |
|-------|--------|
| Installing package on only one server | Task requires installation on all app servers |
| Forgetting to enable service | Service will not start after reboot |
| Typing wrong package name | Installation fails |
| Not using sudo | Permission denied |

---

# Key Concepts Learned

- Linux package installation using YUM
- Service management with systemctl
- Enabling services to start on boot
- Managing services across multiple servers
- Backend dependency configuration

---

# Conclusion

The **NSCD package** was successfully installed on all application servers in the Stratos Datacenter. The **NSCD service was started and enabled**, ensuring it will automatically run during system boot and provide caching support for system name service lookups.
