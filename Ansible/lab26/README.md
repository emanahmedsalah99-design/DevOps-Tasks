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
sudo dnf install ansible-core -y
ansible --version
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Install%20Ansible.png?raw=true)
### 2. Generate SSH Key
```bash
ssh-keygen
# Press Enter for all prompts
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Generate%20SSH%20Key.png?raw=true)
### 3. Copy Public Key to Managed Node
```bash
ssh-copy-id ememty@192.168.100.27
ssh ememty@192.168.100.27   # Test login
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Copy%20Public%20Key%20to%20Managed%20Node.png?raw=true)

![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/%23%20Test%20login.png?raw=true)
### 4. Create Inventory
```bash
sudo nano /etc/ansible/hosts
```

Add:
```bash
[managed]
node1 ansible_host=192.168.100.27 ansible_user=ememty
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Add.png?raw=true)
### 5. Test Connection
```bash
ansible managed -m ping
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Test%20Connection.png?raw=true)
### 6. Run Ad-Hoc Command (Check Disk Space)
```bash
ansible managed -a "df -h"
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab26/Screenshots/Run%20Ad-Hoc%20Command%20(Check%20Disk%20Space).png?raw=true)

## Conclusion
- Ansible installed and verified
- Passwordless SSH to managed node established
- Inventory configured and connection tested
- Ad-hoc command executed successfully ✅
---



