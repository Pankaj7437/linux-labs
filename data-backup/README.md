# Day 06 – Create and Transfer a Compressed Archive

## Objective
Create a compressed archive of the `/data/yousuf` directory and move it to the `/home` directory on the Jump Host server.

---

## Task Description

The Jump Host server contains a directory `/data` used by developers to store non-confidential data.  
Developer **yousuf** requested a compressed copy of their data stored in:

```
/data/yousuf
```

The system administrator must:

1. Create a compressed archive named `yousuf.tar.gz`.
2. Transfer the archive to the `/home` directory on the Jump Host.

---

## Steps Performed

### Navigate to the developer directory

```bash
cd /data/yousuf
```

Check files:

```bash
ls
```

Example files:

```
nautilus1.txt
nautilus2.txt
nautilus3.txt
```

---

### Create compressed archive

```bash
tar -cvzf yousuf.tar.gz *.txt
```

Explanation:

| Option | Meaning |
|------|------|
| c | Create archive |
| v | Verbose output |
| z | Compress using gzip |
| f | Specify archive file name |

---

### Transfer archive to `/home`

```bash
sudo cp yousuf.tar.gz /home
```

---

### Verification

```bash
cd /home
ls
```

Expected output:

```
yousuf.tar.gz
```

---

## Summary

A compressed archive of the `/data/yousuf` directory was created using the `tar` command and successfully transferred to the `/home` directory on the Jump Host server.
