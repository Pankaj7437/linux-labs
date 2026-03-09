# Day 15 – Synchronize Timezone Across App Servers

## Objective
Configure all Nautilus application servers to use the timezone **Asia/Kuala_Lumpur**.

## Command

```bash
sudo timedatectl set-timezone Asia/Kuala_Lumpur
```

## Verification

```bash
timedatectl
```

Expected output should show:

```
Time zone: Asia/Kuala_Lumpur
```

## Result
The timezone on all application servers was synchronized with the datacenter timezone.
