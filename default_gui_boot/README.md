# Day 14 – Configure Default Runlevel for GUI Boot

## Objective
Configure all application servers to boot into **GUI mode by default** without rebooting the system.

## Command

```bash
sudo systemctl set-default graphical.target
```

## Verification

```bash
systemctl get-default
```

Expected output:

```
graphical.target
```

## Result

The default system target was successfully configured to `graphical.target`, enabling GUI mode on system boot.
