# Day 13 – Configure Crontab Access Control

## Objective
Configure cron access control on App Server 1 by allowing the user **james** and denying the user **eric**.

## Steps

Allow user james

```bash
sudo vi /etc/cron.allow
```

Add:

```
james
```

Deny user eric

```bash
sudo vi /etc/cron.deny
```

Add:

```
eric
```

## Result

The user **james** is permitted to manage cron jobs while **eric** is denied access.
