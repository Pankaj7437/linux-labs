# EC2 Boot Failure Recovery (fstab Issue)

## Overview
This project documents a real-world Linux/Cloud troubleshooting scenario where a RHEL EC2 instance failed to boot and entered emergency mode due to an invalid filesystem mount entry inside `/etc/fstab`.

The issue caused:
- SSH connection failure
- EC2 status check failure
- Emergency mode boot
- Root account locked message

The recovery was performed by attaching the broken EBS volume to a helper Ubuntu EC2 instance and manually fixing the filesystem configuration.

---

# Problem Symptoms

During boot, the instance showed errors like:

```text
Timed out waiting for device /dev/rcat/redhat
Dependency failed for local-fs.target
You are in emergency mode
Cannot open access to console, the root account is locked
````

As a result:

* EC2 instance became inaccessible
* SSH stopped working
* System entered emergency mode

---

# Root Cause

An invalid/broken mount entry existed inside:

```bash
/etc/fstab
```

The system attempted to mount a non-existent device during boot:

```text
/dev/rcat/redhat
```

Since the device did not exist, systemd failed to complete the boot process.

---

# Recovery Architecture

Broken RHEL EC2
↓
Detach Root EBS Volume
↓
Attach Volume to Helper Ubuntu EC2
↓
Mount Filesystem
↓
Fix /etc/fstab
↓
Reattach Volume
↓
Successful Boot

---

# Recovery Steps

## 1. Stop Broken EC2

Stopped the affected EC2 instance from AWS Console.

---

## 2. Detach Root Volume

Detached the root EBS volume from the broken instance.

---

## 3. Attach Volume to Helper EC2

Attached the volume to a healthy Ubuntu EC2 instance for recovery.

Verified attached disk:

```bash
lsblk
```

---

## 4. Identify Correct Partition

Located the root filesystem partition:

```bash
/dev/nvme1n1p3
```

---

## 5. Mount Filesystem

```bash
sudo mkdir /mnt/recovery
sudo mount /dev/nvme1n1p3 /mnt/recovery
```

---

## 6. Inspect fstab

```bash
cat /mnt/recovery/etc/fstab
```

Found invalid mount entry.

---

## 7. Fix Configuration

Edited the file:

```bash
sudo nano /mnt/recovery/etc/fstab
```

Commented/removing broken mount entry:

```fstab
# /dev/rcat/redhat /info xfs defaults 0 0
```

---

## 8. Unmount Filesystem

```bash
sudo umount /mnt/recovery
```

---

## 9. Reattach Volume

Reattached the EBS volume back to the original EC2 instance as the root volume.

---

## 10. Boot Success

Started the instance successfully and verified:

* SSH access restored
* System boot completed normally
* EC2 status checks passed

---

# Key Linux/Cloud Concepts Learned

* Linux boot process
* systemd dependencies
* emergency mode
* `/etc/fstab`
* filesystem mounts
* EBS recovery
* EC2 troubleshooting
* root volume recovery
* cloud infrastructure debugging

---

# Tools Used

* AWS EC2
* EBS Volumes
* Ubuntu EC2
* RHEL EC2
* Linux CLI
* systemd
* lsblk
* mount
* nano

---

# Lessons Learned

* Incorrect `fstab` entries can completely break Linux boot.
* Cloud recovery often involves offline disk repair.
* Understanding Linux boot flow is critical for DevOps/SRE roles.
* Real debugging skills come from fixing production-like failures.

---

# Author

Pankaj Sharma
DevOps & Cloud Enthusiast

```
```
