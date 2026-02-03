# Lab 26: Initial Ansible Configuration and Ad-Hoc Execution

## Objective
- Configure Ansible on a control node
- Establish passwordless SSH to a managed node
- Run ad-hoc commands

---

## Environment
- **Control Node:** RHEL 10 VM, user `ememty`  
- **Managed Node:** Same VM (for lab), IP: `192.168.100.27`, user `ememty`  
- **Ansible version:** core 2.16.x  
- SSH enabled between control and managed nodes

---

## Steps & Commands

### 1. Install Ansible
```bash
sudo dnf clean all
sudo dnf makecache
sudo dnf install ansible-core -y
ansible --version
``` 
### 1. Install Ansible
```bash
Generate SSH Key
``` 
