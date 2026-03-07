# Day 12 – Secure File Transfer using SCP

## Objective
Transfer the file `/tmp/nautilus.txt.gpg` from the jump host to **App Server 1** in the directory `/home/data`.

## Command

```bash
scp /tmp/nautilus.txt.gpg tony@stapp01:/home/data
```

## Explanation

- `scp` is used to securely copy files between systems over SSH.
- The file was transferred from the jump host to **App Server 1**.
- The destination directory was `/home/data`.

## Result

The file `nautilus.txt.gpg` was successfully transferred to the application server.
