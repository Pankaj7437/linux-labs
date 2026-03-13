# DevOps Collaborative Directory Setup – Stratos Datacenter

## Objective
Create a secure collaborative directory **/devops/data** on **App Server 2 (stapp02)** that can be accessed only by the **devops group**.  
Other users or groups should not have any access.

---

## Requirements

- Directory path: `/devops/data`
- Group owner: `devops`
- Permissions:
  - User → read, write, execute
  - Group → read, write, execute
  - Others → no access
- Files created inside the directory should inherit the **devops group ownership**.

---

## Steps Performed

### 1. Login to App Server 2
```
ssh steve@stapp02
```

### 2. Create the directory
```
sudo mkdir -p /devops/data
```

### 3. Set group ownership
```
sudo chown :devops /devops/data
```

### 4. Set directory permissions
```
sudo chmod 770 /devops/data
```

### 5. Enable group inheritance (setgid)
This ensures that all files created inside the directory belong to the **devops group**.

```
sudo chmod g+s /devops/data
```

---

## Verification

Check directory ownership and permissions:

```
ls -ld /devops/data
```

Expected output:

```
drwxrws--- 2 root devops ...
```

Explanation:

- `rwx` → owner permissions
- `rwx` → group permissions
- `---` → others have no access
- `s` → setgid enabled (files inherit devops group)

---

## Result

- Secure collaborative directory created
- Only **devops group** has access
- Other users/groups cannot access the directory
- Files inside the directory inherit **devops group ownership**
