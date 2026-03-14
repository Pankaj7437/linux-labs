# Linux Security Investigation Lab

## Objective
During a routine security audit on the Nautilus App Server, malicious content was detected in the website code. The task is to locate all `.php` files inside `/var/www/html/media` and copy them to `/media` while preserving their parent directory structure without copying the entire directory.

---

# Environment Details

| Parameter | Value |
|----------|------|
| Server | App Server 3 |
| Source Path | /var/www/html/media |
| Destination Path | /media |
| File Type | .php |
| Requirement | Preserve parent directory structure |

---

# Initial Directory Structure

Example structure of the source directory:

```
/var/www/html/media
├── images
│   └── upload.php
├── videos
│   └── process.php
└── index.php
```

---

# Step 1 – Connect to App Server

```bash
ssh banner@stapp03
```

This connects to **App Server 3** where the investigation task will be performed.

---

# Step 2 – Identify PHP Files

```bash
find /var/www/html/media -type f -name "*.php"
```

### Explanation

| Command | Purpose |
|--------|---------|
| find | Searches files recursively |
| /var/www/html/media | Target directory |
| -type f | Filters only files |
| -name "*.php" | Matches PHP extension |

---

# Step 3 – Copy Files with Parent Directory Structure

```bash
cd /
find var/www/html/media -type f -name "*.php" -exec cp --parents {} /media \;
```

### Explanation

| Command Component | Description |
|------------------|-------------|
| cd / | Move to root directory |
| find | Search command |
| -exec | Execute command on search results |
| cp --parents | Preserve original directory structure |
| {} | Represents the found file |
| /media | Destination directory |

---

# Resulting Directory Structure

After copying, the files appear as:

```
/media
└── var
    └── www
        └── html
            └── media
                ├── images
                │   └── upload.php
                ├── videos
                │   └── process.php
                └── index.php
```

---

# Verification

```bash
ls -R /media
```

This command recursively lists all copied files to confirm the structure.

---

# Common Mistakes

| Mistake | Problem |
|-------|---------|
| Using `cp -r` | Copies the entire directory |
| Not using `--parents` | Directory structure lost |
| Forgetting `-type f` | Directories may be included |
| Wrong source path | Files copied incorrectly |

---

# Commands Cheat Sheet

```bash
ssh banner@stapp03

find /var/www/html/media -type f -name "*.php"

cd /
find var/www/html/media -type f -name "*.php" -exec cp --parents {} /media \;

ls -R /media
```

---

# Key Linux Concepts Used

- `find` command for recursive file search
- Filtering files by extension
- `cp --parents` for preserving directory hierarchy
- Recursive directory verification

---

# Conclusion

Using the `find` command along with `cp --parents`, all `.php` files were successfully identified and copied from `/var/www/html/media` to `/media` while maintaining the original directory structure. This ensures safe investigation of potentially malicious files without affecting the original website directory.
