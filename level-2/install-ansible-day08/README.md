# Day 8 – Install Ansible using pip3

## Objective
Install **Ansible version 4.10.0** on the jump host using `pip3` and ensure it is globally accessible.

## Steps

Install pip3

```bash
sudo yum install -y python3-pip
```

Install Ansible

```bash
sudo pip3 install ansible==4.10.0
```

Verify installation

```bash
ansible --version
```

## Result

Ansible version **4.10.0** was successfully installed and is available globally for all users.
