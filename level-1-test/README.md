# Linux level 1 Test

This repository documents solutions for multiple system administration and DevOps tasks performed on the **Nautilus infrastructure within Stratos Datacenter**.

---

# Task 1 – Cron Job Setup on All App Servers

## Objective
Install the `cronie` package and configure a test cron job on all application servers.

## Steps

```
sudo yum install cronie -y
sudo systemctl start crond
sudo systemctl enable crond
```

Add cron job for root user:

```
sudo crontab -e
```

Add the following line:

```
*/5 * * * * echo hello > /tmp/cron_text
```

---

# Task 2 – Modify User Expiration and Copy Files

## Objective
Update the expiration date of user **kirsty** and copy all files owned by this user from `/home/usersdata` to `/news` while preserving ownership.

## Commands

```
sudo chage -E 2027-01-28 kirsty
```

```
sudo find /home/usersdata -type f -user kirsty -exec cp --preserve=ownership {} /news/ \;
```

---

# Task 3 – Secure File Transfer

## Objective
Copy a secure file from the jump server to **App Server 2**.

## Command

```
scp /tmp/nautilus.txt.gpg steve@stapp02:/home/code
```

---

# Task 4 – Fix Directory Permissions

## Objective
Change permissions of `/usr/share/data` directory on App Server 2.

## Command

```
sudo chmod 700 /usr/share/data
```

---

# Task 5 – Create Web Server Test Page

## Objective
Create a test web page on App Server 2.

## Steps

Navigate to directory:

```
cd /var/www/html
```

Create file:

```
sudo vi index.html
```

Add content:

```
Welcome to the KKE labs!
```

---

# Task 6 – Create Archive from Storage Server

## Objective
Create a compressed archive of `/data/kirsty` and move it to `/home`.

## Commands Used

```
cd /data
sudo tar -czvf kirsty.tar.gz kirsty
sudo mv kirsty.tar.gz /home/
```

## Note

Although the archive was successfully created and moved to `/home`, the task validation in the lab environment still marked this step as incorrect. The commands executed follow standard Linux practices, and the archive was verified manually. The exact validation condition expected by the environment may differ.

---

# Task 7 – Modify File Content

## Objective
Edit `/home/BSD.txt` and generate modified outputs.

### Remove lines containing "code"

```
grep -v "code" /home/BSD.txt | sudo tee /home/BSD_DELETE.txt
```

### Replace word "and" with "them"

```
sed 's/\band\b/them/g' /home/BSD.txt | sudo tee /home/BSD_REPLACE.txt
```

---

# Task 8 – Remove Package from All App Servers

## Objective
Remove `logrotate` package from all app servers.

```
sudo yum remove logrotate -y
```

---

# Task 9 – Limit Maximum Processes for nfsuser

## Objective
Restrict number of processes for the user `nfsuser`.

Edit file:

```
/etc/security/limits.conf
```

Add:

```
nfsuser soft nproc 1025
nfsuser hard nproc 2025
```

---

# Task 10 – Install sqlite Package

## Objective
Install the `sqlite` package on all app servers.

```
sudo yum install sqlite -y
```

Verify:

```
sqlite3 --version
```

---

# Conclusion

These exercises simulate real-world DevOps and Linux system administration scenarios including:

- Cron job automation
- File transfer and archiving
- Directory permissions management
- Package installation and removal
- User account management
- Resource limitation

They provide practical experience in managing multiple servers within a production-like environment.
