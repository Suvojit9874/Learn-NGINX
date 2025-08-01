<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>


# Basic Linux Commands

**System Navigation and File Management**

- List files/folders:

```
ls
```

- List detailed info (including hidden):

```
ls -la
```

- Change directory:

```
cd /path/to/directory
```

- Go back one directory:

```
cd ..
```

- Print current directory:

```
pwd
```

- Copy files:

```
cp source.txt destination.txt
```

- Move/rename files:

```
mv old.txt new.txt
```

- Remove files:

```
rm file.txt
```

- Remove directories:

```
rm -r directory
```


**System and Process Information**

- Show running processes:

```
ps aux
```

- Monitor live processes:

```
top
```

- Kill a process by PID:

```
kill PID
```

- Show disk usage:

```
df -h
```

- Show memory usage:

```
free -h
```

- Shutdown/reboot:

```
sudo shutdown now
sudo reboot
```


**Software Installation and System Updates (Debian/Ubuntu)**

- Update package lists:

```
sudo apt update
```

- Upgrade installed packages:

```
sudo apt upgrade
```

- Install a package (e.g., vim):

```
sudo apt install vim
```

- Remove a package:

```
sudo apt remove package_name
```


**Managing Services**

- Start/restart/stop daemon:

```
sudo systemctl start service
sudo systemctl restart service
sudo systemctl stop service
```

- Enable/disable service on boot:

```
sudo systemctl enable service
sudo systemctl disable service
```

- Check service status:

```
sudo systemctl status service
```


# nginx Commands: From Download to Management

**1. Download/Install nginx (Debian/Ubuntu)**

- Update and install:

```
sudo apt update
sudo apt install nginx
```


**2. Start/Enable/Check nginx**

- Start nginx:

```
sudo systemctl start nginx
```

- Enable nginx on boot:

```
sudo systemctl enable nginx
```

- Stop/restart nginx:

```
sudo systemctl stop nginx
sudo systemctl restart nginx
```

- Check nginx status:

```
sudo systemctl status nginx
```


**3. Basic Configuration**

- nginx config file (main):

```
/etc/nginx/nginx.conf
```

- Site configurations:

```
/etc/nginx/sites-available/
/etc/nginx/sites-enabled/
```

- Test configuration syntax:

```
sudo nginx -t
```

- Reload nginx after config change:

```
sudo systemctl reload nginx
```


# Key Docker Commands (Linux Host Related Only)

**Install Docker (Debian/Ubuntu)**

- Update and install prerequisites:

```
sudo apt update
sudo apt install ca-certificates curl gnupg
```

- Install Docker (official repos recommended; see docs for step-by-step):

```
curl -fsSL https://get.docker.com | sudo bash
```


**Docker Basic Commands**

- Check Docker service status:

```
sudo systemctl status docker
```

- Start Docker:

```
sudo systemctl start docker
```

- Enable Docker at startup:

```
sudo systemctl enable docker
```

- List Docker containers:

```
sudo docker ps
sudo docker ps -a
```

- Pull an image:

```
sudo docker pull nginx
```

- Run a container:

```
sudo docker run --name mynginx -d -p 8080:80 nginx
```

- Stop/remove a container:

```
sudo docker stop mynginx
sudo docker rm mynginx
```

- Show system disk usage by Docker:

```
sudo docker system df
```

- Remove unused images/containers:

```
sudo docker system prune
```



# 🐧 Linux, 🖧 Nginx & 🐳 Docker Commands Cheat Sheet

---

## 🔧 Basic Linux Commands

| Task | Command |
|------|---------|
| Update package list | `sudo apt update` |
| Upgrade installed packages | `sudo apt upgrade` |
| Install a package | `sudo apt install <package>` |
| Check current directory | `pwd` |
| List files | `ls -l` or `ls -la` |
| Change directory | `cd <path>` |
| Create a file | `touch <filename>` |
| Edit file using vim | `vim <filename>` |
| Create directory | `mkdir <dirname>` |
| Delete file/directory | `rm <file>` / `rm -r <dir>` |
| Show current user | `whoami` |
| Show system info | `uname -a` |
| Check disk space | `df -h` |
| Show running processes | `ps aux` |
| Kill a process | `kill -9 <PID>` |
| View logs | `tail -f /var/log/syslog` |

---

## 📦 Nginx Commands

| Task | Command |
|------|---------|
| Install Nginx | `sudo apt install nginx` |
| Start Nginx | `sudo systemctl start nginx` |
| Stop Nginx | `sudo systemctl stop nginx` |
| Restart Nginx | `sudo systemctl restart nginx` |
| Reload config | `sudo systemctl reload nginx` |
| Enable on boot | `sudo systemctl enable nginx` |
| Check status | `sudo systemctl status nginx` |
| Config location | `/etc/nginx/nginx.conf` |
| Default site config | `/etc/nginx/sites-available/default` |
| Test config for errors | `sudo nginx -t` |
| Manually reload Nginx | `sudo nginx -s reload` |

---

## 🧰 Vim Basics

| Task | Command |
|------|---------|
| Enter insert mode | `i` |
| Save and exit | `Esc` then `:wq` |
| Exit without saving | `Esc` then `:q!` |
| Delete a line | `dd` |
| Copy/paste | `yy` (yank), `p` (paste) |

---

## 🐳 Docker Commands

| Task | Command |
|------|---------|
| List all containers | `docker ps -a` |
| Run a container with shell | `docker run -it ubuntu /bin/bash` |
| Start/stop container | `docker start <id>` / `docker stop <id>` |
| Enter running container | `docker exec -it <id> bash` |
| Copy file to container | `docker cp file.txt <id>:/path/in/container` |
| Create Docker image | `docker build -t my-image .` |
| Remove container/image | `docker rm <id>` / `docker rmi <image>` |

---


