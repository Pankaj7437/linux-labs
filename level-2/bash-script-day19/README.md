# Automating Website Backup with Bash Script

## Objective

Create a bash script named **media_backup.sh** to automate backup of the website media directory on **App Server 2** and store it locally as well as on the **Nautilus Storage Server**.

The script must:

- Create a zip archive of `/var/www/html/media`
- Save the archive in `/backup`
- Copy the archive to the Nautilus Storage Server `/backup`
- Run without asking for a password
- Avoid using `sudo` inside the script

---

# Initial Setup

Logged into **App Server 2**.

```bash
ssh steve@stapp02
```

---

# Step 1 – Verify zip package

Checked whether zip package was installed.

```bash
zip -v
```

Zip was already installed on the server.

---

# Step 2 – Inspect website directory

Verified the media directory.

```bash
ls /var/www/html/media
```

Output:

```
index.html
```

---

# Step 3 – Initial Backup Attempt

Tried creating the archive inside the media directory.

```bash
zip xfusioncorp_media.zip /var/www/html/media
```

Error:

```
Permission denied
```

This happened because the directory required elevated privileges.

Temporary test with sudo:

```bash
sudo zip xfusioncorp_media.zip /var/www/html/media
```

Archive was created successfully, but it created a **root owned file**, which could not be removed normally.

```
Permission denied
```

Removed it using:

```bash
sudo rm -rf xfusioncorp_media.zip
```

---

# Step 4 – Create Scripts Directory

The lab required placing the script inside `/scripts`.

```bash
sudo mkdir -p /scripts
sudo chown steve:steve /scripts
```

---

# Step 5 – Setup Passwordless SSH

The backup must be copied to the storage server **without password prompt**.

Generated SSH key.

```bash
ssh-keygen
```

Copied the key to the storage server.

```bash
ssh-copy-id natasha@ststor01
```

Verified connection.

```bash
ssh natasha@ststor01
```

---

# Step 6 – Create Backup Script

Script location:

```
/scripts/media_backup.sh
```

Created the script:

```bash
vim /scripts/media_backup.sh
```

Script content:

```bash
#!/bin/bash

zip -r /backup/xfusioncorp_media.zip /var/www/html/media

scp /backup/xfusioncorp_media.zip natasha@ststor01:/backup/
```

---

# Step 7 – Make Script Executable

```bash
chmod +x /scripts/media_backup.sh
```

---

# Step 8 – Execute Script

Ran the script.

```bash
/scripts/media_backup.sh
```

Output:

```
adding: var/www/html/media/
adding: var/www/html/media/index.html
```

Archive successfully created.

---

# Step 9 – Verify Local Backup

```bash
ls /backup
```

Output:

```
xfusioncorp_media.zip
```

---

# Step 10 – Verify Storage Server Backup

Logged into storage server.

```bash
ssh natasha@ststor01
```

Checked backup directory.

```bash
ls /backup
```

Output:

```
xfusioncorp_media.zip
```

Backup was successfully transferred.

---

# Mistakes During the Process

### Typo while generating SSH key

Incorrect command:

```
ssh key-gen
```

Correct command:

```
ssh-keygen
```

---

### Typo while checking backup directory

Incorrect path:

```
/bakup
```

Correct path:

```
/backup
```

---

### Creating archive with sudo

Initially used:

```
sudo zip
```

This created root-owned files, which caused permission issues.

The final script avoids using sudo as required by the task.

---

# Final Script

```bash
#!/bin/bash

zip -r /backup/xfusioncorp_media.zip /var/www/html/media

scp /backup/xfusioncorp_media.zip natasha@ststor01:/backup/
```

---

# Key Concepts Learned

- Automating tasks using Bash scripts
- Creating compressed backups using `zip`
- Using `scp` for secure file transfer
- Configuring **passwordless SSH**
- Proper file permissions management

---

# Conclusion

The backup script successfully automates the process of archiving the website media directory and transferring the backup to the Nautilus Storage Server without requiring manual intervention.
