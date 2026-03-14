# Linux Package Management Lab – Install Redis on Application Servers

## Objective
The Nautilus DevOps team needs to install the **Redis package** on all application servers in the infrastructure. Redis is an in-memory data structure store commonly used as a database, cache, and message broker.

The task is to install the **redis package on all app servers** in the Nautilus environment.

---

# Environment Details

| Parameter | Value |
|----------|------|
| Infrastructure | Nautilus |
| Server Type | Application Servers |
| Package Manager | YUM / DNF |
| Package to Install | redis |
| Target Servers | stapp01, stapp02, stapp03 |

---

# Initial Situation

The Redis package is not installed on the application servers.  
The goal is to install the Redis package using the Linux package manager so that the application servers can use Redis for caching and data storage.

---

# Step 1 – Connect to Application Servers

Login to each application server from the jump host.

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

Explanation:

| Command | Purpose |
|-------|---------|
| ssh | Secure shell login |
| user@hostname | Connect to specific server |

---

# Step 2 – Install Redis Package

Install the Redis package using the package manager.

```bash
sudo yum install redis -y
```

Explanation:

| Command | Purpose |
|-------|---------|
| yum install | Installs software packages |
| redis | Redis package name |
| -y | Automatically confirm installation |

---

# Step 3 – Verify Installation

Check whether Redis has been installed successfully.

```bash
rpm -qa | grep redis
```

Example output:

```
redis-6.x.x.el9.x86_64
```

Explanation:

| Command | Purpose |
|-------|---------|
| rpm -qa | Lists installed packages |
| grep redis | Filters redis package |

---

# Alternative Verification Method

You can also verify using:

```bash
redis-server --version
```

Example output:

```
Redis server v=6.x.x
```

---

# Commands Cheat Sheet

```bash
ssh tony@stapp01
sudo yum install redis -y

ssh steve@stapp02
sudo yum install redis -y

ssh banner@stapp03
sudo yum install redis -y

rpm -qa | grep redis
```

---

# Common Mistakes

| Mistake | Problem |
|-------|--------|
| Installing package on only one server | Task requires installation on all app servers |
| Typing wrong package name | Installation fails |
| Not using sudo | Permission denied |
| Network repo issues | Package cannot be downloaded |

---

# Key Concepts Learned

- Linux package management
- Installing packages using yum
- Verifying installed packages
- Managing software across multiple servers
- Basic DevOps infrastructure management

---

# Conclusion

The Redis package was successfully installed on all Nautilus application servers using the YUM package manager. Installation was verified using the RPM query command, ensuring that Redis is available for application usage across the infrastructure.
