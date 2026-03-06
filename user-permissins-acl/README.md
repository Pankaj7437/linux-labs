# Day 10 – Manage File ACL Permissions

## Objective
Configure Access Control Lists on `/etc/hostname` on Nautilus App Server 2.

## Steps

Set owner and group to root

```bash
sudo chown root:root /etc/hostname
```

Give others read-only permission

```bash
sudo chmod o=r /etc/hostname
```

Remove permissions for user javed

```bash
sudo setfacl -m u:javed:0 /etc/hostname
```

Grant read-only permission to user jerome

```bash
sudo setfacl -m u:jerome:r /etc/hostname
```

Verify ACL

```bash
getfacl /etc/hostname
```

## Summary
ACL rules were configured to restrict access to `/etc/hostname` while allowing specific read permissions for authorized users.
