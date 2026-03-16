# Linux Repository Management Lab – Configure Local YUM Repository

## Objective
The Nautilus production support and security teams decided to maintain packages using **local YUM repositories** instead of external repositories.

The goal of this task is to:

- Configure a **local YUM repository** on the **Nautilus Backup Server**
- Use packages stored in `/packages/downloaded_rpms`
- Create a repository named **yum_local**
- Install the **samba** package from this newly created repository

---

# Environment Details

| Parameter | Value |
|----------|------|
| Infrastructure | Nautilus |
| Server | Nautilus Backup Server |
| Package Manager | YUM / DNF |
| Repository Name | yum_local |
| Repository ID | yum_local |
| Package Location | /packages/downloaded_rpms |
| Package to Install | samba |

---

# Initial Situation

The required RPM packages are already present on the backup server at:

```
/packages/downloaded_rpms
```

However, these packages cannot be installed directly using YUM unless a **local repository is configured**.

---

# Step 1 – Connect to Nautilus Backup Server

Login from the jump host.

```bash
ssh clint@stbkp01
```

---

# Step 2 – Verify Package Directory

Check the available RPM packages.

```bash
ls /packages/downloaded_rpms
```

Example output:

```
samba.rpm
libsamba.rpm
samba-common.rpm
```

---

# Step 3 – Create Local YUM Repository Configuration

Navigate to the YUM repository configuration directory.

```bash
cd /etc/yum.repos.d
```

Create a new repo configuration file.

```bash
sudo vi yum_local.repo
```

Add the following configuration:

```
[yum_local]
name=Local YUM Repository
baseurl=file:///packages/downloaded_rpms
enabled=1
gpgcheck=0
```

### Explanation

| Parameter | Purpose |
|----------|---------|
| yum_local | Repository ID |
| name | Repository description |
| baseurl | Location of RPM packages |
| enabled=1 | Enables repository |
| gpgcheck=0 | Disables GPG verification |

---

# Step 4 – Refresh YUM Cache

Clear and rebuild repository metadata.

```bash
sudo yum clean all
sudo yum repolist
```

Expected output should display:

```
yum_local
```

This confirms that the repository is successfully configured.

---

# Step 5 – Install Samba Package

Install the samba package using the new repository.

```bash
sudo yum install samba -y
```

---

# Step 6 – Verify Installation

Confirm that samba has been installed.

```bash
rpm -qa | grep samba
```

Example output:

```
samba-x.x.x.el9.x86_64
```

---

# Commands Cheat Sheet

```bash
ssh clint@stbkp01

ls /packages/downloaded_rpms

cd /etc/yum.repos.d

sudo vi yum_local.repo

sudo yum clean all
sudo yum repolist

sudo yum install samba -y

rpm -qa | grep samba
```

---

# Common Mistakes

| Mistake | Problem |
|-------|--------|
| Incorrect baseurl path | YUM cannot locate packages |
| Forgetting `file:///` prefix | Repository will not work |
| Not refreshing YUM cache | Repo not detected |
| Wrong repository ID | Task validation fails |

---

# Key Concepts Learned

- Local YUM repository configuration
- Package management using YUM
- Installing software from local repositories
- Repository metadata management
- Linux package distribution control

---

# Conclusion

A **local YUM repository named `yum_local`** was successfully configured on the Nautilus Backup Server using packages stored at `/packages/downloaded_rpms`. The repository was verified and used to install the **samba package**, ensuring that local package management is functioning correctly.
