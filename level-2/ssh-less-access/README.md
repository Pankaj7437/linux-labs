# DevOps Task: Setup Password-less SSH from Jump Host

## Task
Configure password-less SSH access from user **thor** on the **jump host** to all application servers in the **Stratos Datacenter** using their respective users.

Automation scripts running on the jump host require SSH access without asking for passwords.

---

## Server Mapping

| Server | Hostname | User |
|------|------|------|
| App Server 1 | stapp01 | tony |
| App Server 2 | stapp02 | steve |
| App Server 3 | stapp03 | banner |

Jump Host User: `thor`

---

## Step 1: Login to Jump Host

ssh thor@jump-host

---

## Step 2: Generate SSH Key

ssh-keygen

Press **Enter** for all prompts to generate the SSH key without passphrase.

Key files created:

~/.ssh/id_rsa  
~/.ssh/id_rsa.pub  

---

## Step 3: Copy SSH Key to App Server 1

ssh-copy-id tony@stapp01

Enter password once to install the key.

---

## Step 4: Copy SSH Key to App Server 2

ssh-copy-id steve@stapp02

---

## Step 5: Copy SSH Key to App Server 3

ssh-copy-id banner@stapp03

---

## Verification

ssh tony@stapp01
exit

ssh steve@stapp02
exit

ssh banner@stapp03
exit

If login works **without password**, the setup is successful.

---

## Tools Used

SSH  
ssh-keygen  
ssh-copy-id  

---

## Result

Password-less SSH authentication was successfully configured from **thor (jump host)** to all **application servers**, enabling automation scripts to run without manual password input.
