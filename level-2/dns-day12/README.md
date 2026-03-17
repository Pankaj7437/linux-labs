# Linux DNS Configuration Lab – Fix DNS Resolution Using Google Public DNS

## Objective

The system administrators observed **intermittent DNS resolution issues** on **App Server 1 (stapp01)** in the Stratos Datacenter.

To resolve this issue, the server must be configured to use **Google Public DNS (IPv4)** as a temporary fix.

---

# Key Concepts

- **DNS (Domain Name System):** Converts domain names (google.com) into IP addresses.
- **/etc/resolv.conf:** Main DNS configuration file in Linux.
- **Nameserver:** Defines which DNS server the system queries.
- **IPv4 DNS:** Uses A records (e.g., 8.8.8.8).

---

# Environment Details

| Parameter | Value |
|---|---|
| Infrastructure | Nautilus |
| Datacenter | Stratos |
| Server | App Server 1 |
| Hostname | stapp01 |
| Config File | /etc/resolv.conf |
| DNS Provider | Google Public DNS |

---

# Step-by-Step Walkthrough

## 1. Connect to App Server

```bash
ssh tony@stapp01
```

---

## 2. Check Current DNS Configuration

```bash
cat /etc/resolv.conf
```

### Observation

- Existing nameserver may point to internal DNS (e.g., `127.x.x.x`)
- System may still resolve domains using IPv6

---

## 3. Configure Google Public DNS (IPv4)

Edit the configuration file:

```bash
sudo vi /etc/resolv.conf
```

Add or update:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### Important

- Place these entries at the **top** (priority matters)
- These are **IPv4 DNS servers**

---

## 4. Verify DNS Resolution

Since `ping` is not available, use:

```bash
getent ahostsv4 google.com
```

Expected output:

```
xxx.xxx.xxx.xxx google.com
```

Or:

```bash
curl -4 google.com
```

---

## 5. Understanding the Output

If curl returns:

```
The document has moved
```

This means:

- DNS resolution ✔ working
- Network connectivity ✔ working
- HTTP redirect (not an error)

---

# DNS Resolution Flow (Conceptual)

```
User Request
   ↓
/etc/nsswitch.conf
   ↓
/etc/hosts → /etc/resolv.conf
   ↓
Google DNS (8.8.8.8)
   ↓
IP Address Returned
```

---

# Commands Cheat Sheet

```bash
ssh tony@stapp01

cat /etc/resolv.conf

sudo vi /etc/resolv.conf

nameserver 8.8.8.8
nameserver 8.8.4.4

getent ahostsv4 google.com

curl -4 google.com
```

---

# Common Mistakes

| Mistake | Problem |
|---|---|
| DNS already working but not updated | Lab still fails |
| IPv6 resolution instead of IPv4 | Requirement mismatch |
| Not adding Google DNS | Task incomplete |
| Wrong file edited | No effect |

---

# Key Learning

- Difference between **working DNS vs required DNS**
- IPv4 vs IPv6 resolution
- DNS priority based on order in resolv.conf
- Real-world DNS troubleshooting

---

# Conclusion

The DNS issue on **App Server 1** was resolved by configuring **Google Public DNS servers (8.8.8.8 and 8.8.4.4)**.  
Even though DNS was previously working, the system is now explicitly configured to use **IPv4-based public DNS**, ensuring reliable and consistent resolution.
