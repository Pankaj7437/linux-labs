# Data Filtering and Relocation on App Server 2

This task addresses a data mix-up on Nautilus App Server 2. User data stored under `/home/usersdata` needed to be filtered and relocated based on file ownership.

## Objective
Identify all files (excluding directories) owned by user **jim** within `/home/usersdata` and copy them to the `/ecommerce` directory while preserving the original directory structure.

## Source & Destination
- **Source Directory:** `/home/usersdata`
- **Destination Directory:** `/ecommerce`
- **Target Owner:** `jim`
- **Server:** App Server 2

## Steps Performed

### 1. Login to App Server 2
```bash
ssh <username>@stapp02
```

### 2. Navigate to the user data directory
```bash
cd /home/usersdata
```

### 3. Locate and copy files owned by user `jim`
```bash
find . -type f -user jim -exec cp --parents {} /ecommerce/ \;
```

### Explanation
- `-type f` selects files only  
- `-user jim` filters by owner  
- `cp --parents` preserves directory structure  
- `/ecommerce` is the destination  

## Verification
```bash
ls -R /ecommerce
```

## Outcome
All files owned by **jim** were successfully filtered and relocated to `/ecommerce` with full directory structure preserved, resolving the data organization issue on App Server 2.
