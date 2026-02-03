# Lab 27: Automated Web Server Configuration Using Ansible Playbooks

## Objective
Automate the installation and configuration of a web server using Ansible

## Environment
- **Control Node:** RHEL 10 with Ansible installed  
- **Managed Node:** RHEL 10, reachable via SSH (`192.168.100.27`)  

## Steps & Commands

### 1. Create Playbook Directory
```bash
mkdir -p ~/ansible_playbooks
cd ~/ansible_playbooks
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab27/Screenshots/Create%20Playbook%20Directory.png?raw=true)
### 2. Write Playbook File
```bash
nano ~/ansible_playbooks/webserver.yml
---
- name: Configure Web Server from Scratch (RHEL 10 safe)
  hosts: managed
  become: yes

  tasks:
    - name: Install Nginx
      dnf:
        name: nginx
        state: present

    - name: Open firewall for port 8080 (permanent)
      firewalld:
        port: 8080/tcp
        permanent: yes
        state: enabled
        immediate: yes

    - name: Remove default server blocks if any
      file:
        path: /etc/nginx/conf.d/default.conf
        state: absent

    - name: Create simple Nginx site config on port 8080
      copy:
        dest: /etc/nginx/conf.d/ansible.conf
        content: |
          server {
              listen 8080;
              server_name localhost;
              location / {
                  root /usr/share/nginx/html;
                  index index.html;
              }
          }

    - name: Deploy custom index.html page
      copy:
        dest: /usr/share/nginx/html/index.html
        content: |
          <html>
          <head><title>Welcome to Ansible Nginx Server</title></head>
          <body>
            <h1>Hello from Ansible on port 8080!</h1>
          </body>
          </html>

    - name: Start and enable Nginx service
      systemd:
        name: nginx
        state: started
        enabled: yes

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab27/Screenshots/Write%20Playbook%20File.png?raw=true)
### 3. Run the Playbook
```bash
ansible-playbook ~/ansible_playbooks/webserver.yml
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab27/Screenshots/Run%20the%20Playbook.png?raw=true)
### 4. Verify
```bash
curl http://192.168.100.27:8080
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab27/Screenshots/Screenshot%202026-02-03%20170525.png?raw=true)
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/Ansible/lab27/Screenshots/Screenshot%202026-02-03%20170439.png?raw=true)
## Conclusion
-Nginx installed and running on managed node
-Custom web page deployed on port 8080
-Playbook is idempotent and reusable

