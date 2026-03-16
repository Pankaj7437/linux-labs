# Linux User Privilege Management – Configure Passwordless Sudo for User

## Objective

The system administrators at xFusionCorp Industries need to upgrade user privileges on all application servers in the **Stratos Datacenter**.

The task requirements are:

- Provide **sudo access** to user `john`
- Configure **password-less sudo**
- Apply the configuration on **all application servers**

---

# Environment Details

| Parameter | Value |
|---|---|
| Infrastructure | Nautilus |
| Datacenter | Stratos |
| Servers | stapp01, stapp02, stapp03 |
| User | john |
| Requirement | Passwordless sudo |

---

# Understanding the Requirement

Linux uses the **sudo mechanism** to allow non-root users to execute commands with elevated privileges.

Sudo configuration is stored in:

```
/etc/sudoers
```

However, best practice is to place custom rules inside:

```
/etc/sudoers.d/
```

This avoids accidental corruption of the main sudoers file.

---

# Step 1 – Connect to Application Servers

Login to each server from the jump host.

Example:

```bash
ssh tony@stapp01
```

Repeat for:

```
stapp02
stapp03
```

---

# Step 2 – Configure Passwordless Sudo

Create a sudo rule for user **john**.

Open sudo configuration safely using:

```bash
sudo visudo -f /etc/sudoers.d/john
```

Add the following rule:

```
john ALL=(ALL) NOPASSWD:ALL
```

Explanation:

| Field | Meaning |
|---|---|
| john | Username |
| ALL | All hosts |
| (ALL) | Can run commands as any user |
| NOPASSWD | No password required |
| ALL | All commands allowed |

---

# Step 3 – Save and Exit

After saving, the rule will immediately take effect.

Repeat this configuration on all app servers.

---

# Step 4 – Verify Configuration

Switch to user john.

```bash
su - john
```

Run a sudo command.

```bash
sudo whoami
```

Expected output:

```
root
```

If no password prompt appears, passwordless sudo is successfully configured.

---

# Commands Cheat Sheet

```bash
ssh tony@stapp01

sudo visudo -f /etc/sudoers.d/john

john ALL=(ALL) NOPASSWD:ALL

su - john
sudo whoami
```

Repeat for:

```
stapp02
stapp03
```

---

# Common Mistakes

| Mistake | Problem |
|---|---|
| Editing `/etc/sudoers` directly | Risk of breaking sudo |
| Syntax error in sudo rule | User cannot run sudo |
| Forgetting `NOPASSWD` | Password prompt still appears |
| Configuring only one server | Task fails validation |

---

# Key Concepts Learned

- Linux privilege escalation
- Sudo configuration
- Passwordless sudo setup
- Best practices using `/etc/sudoers.d`
- Multi-server administration

---

# Conclusion

The user **john** was successfully granted **passwordless sudo privileges** on all application servers in the Stratos Datacenter. This allows the user to execute administrative commands without entering a password while maintaining proper privilege management.
