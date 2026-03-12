# Update Message of the Day (MOTD) on All App Servers

## Task
During the monthly compliance meeting, it was discovered that several servers in the **Stratos Datacenter** did not have a valid login banner. The security team provided an approved template that must be displayed to users upon successful login.

The template file is available on the **jump host** at:

```
/home/thor/nautilus_banner
```

The task was to update the **message of the day (MOTD)** on all Nautilus application servers using this template.

---

# Step 1 – Verify Banner Template on Jump Host

```
cat /home/thor/nautilus_banner
```

This file contains the approved login banner.

---

# Step 2 – Copy Banner to App Server 1

```
scp /home/thor/nautilus_banner tony@stapp01:/tmp
```

Login to the server:

```
ssh tony@stapp01
```

Update MOTD:

```
sudo cp /tmp/nautilus_banner /etc/motd
```

Logout:

```
exit
```

---

# Step 3 – Copy Banner to App Server 2

```
scp /home/thor/nautilus_banner steve@stapp02:/tmp
```

Login:

```
ssh steve@stapp02
```

Update MOTD:

```
sudo cp /tmp/nautilus_banner /etc/motd
```

---

# Step 4 – Copy Banner to App Server 3

```
scp /home/thor/nautilus_banner banner@stapp03:/tmp
```

Login:

```
ssh banner@stapp03
```

Update MOTD:

```
sudo cp /tmp/nautilus_banner /etc/motd
```

---

# Verification

Logout and login again to any application server:

```
ssh tony@stapp01
```

The approved banner message should now appear after successful login.

---

# Result

✔ Banner template successfully applied  
✔ MOTD updated on all Nautilus application servers  
✔ Compliance requirement satisfied
