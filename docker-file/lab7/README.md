# Lab 7 – Docker Volume and Bind Mount with Nginx


## Objective
Understand the difference between Docker Volumes and Bind Mounts by running an Nginx container, persisting logs using a volume, and serving a custom HTML page from the host machine using a bind mount.

## Environment
- RHEL 10
- Nginx
- Docker

## Steps & Commands

### Step 1: Create Docker Volume for Nginx Logs
```bash
docker volume create nginx_logs
docker volume ls
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/1-Docker%20Volume.png?raw=true)
### Step 2: Create Bind Mount Directory 
```bash
mkdir -p nginx-bind/html
echo "hello from bind mount" > nginx-bind/html/nginx.html
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/2-Bind%20Mount.png?raw=true)
### Step 3: Run Nginx Container with Volume and Bind Mount
```bash
docker run -d --name nginx-lab7 -p 8096:80 --mount type=volume,source=nginx_logs,target=/var/log/nginx --mount type=bind,source=$(pwd)/nginx-bind/html,target=/usr/share/nginx/html nginx
curl http://localhost:8096
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/3-Run%20Nginx%20Container.png?raw=true)
### Step 5: Modify HTML File on Host
```bash
echo "hello after change" > nginx-bind/html/nginx.html
curl http://localhost:8098
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/4-index.html.png?raw=true)
### Step 6: Verify Logs Stored in Docker Volume
```bash
sudo ls /var/lib/docker/volumes/nginx_logs/_data

```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/Logs.png?raw=true)
### Step 7: Cleanup
```bash
docker rm -f nginx-lab7
docker volume rm nginx_logs
docker ps -a
```
![Repository Cloned](https://github.com/emanahmedsalah99-design/DevOps-Tasks/blob/main/docker-file/lab7/Screenshots/Cleanup.png?raw=true)
## Conclusion
- Docker Volume is used to persist container data (Nginx logs).
- Bind Mount allows the container to use files directly from the host.
- Changes in bind-mounted files are reflected immediately inside the container.

