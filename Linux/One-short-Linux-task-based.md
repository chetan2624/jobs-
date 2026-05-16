<!-- Typing SVG Banner -->
[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=28&pause=1000&color=00C9FF&background=0D1117&center=true&vCenter=true&width=900&lines=🐧+Linux+Practical+Mastery+Guide;Real-World+Tasks+%7C+Real+Skills;From+Backup+to+Deployment+%7C+Let's+Build!;System+Admin+%7C+DevOps+%7C+Cloud+%7C+Scripting)](https://git.io/typing-svg)

<div align="center">

# 🐧 Linux Practical Mastery — Task-Based README

> **"Don't just read Linux. Live it."**
> A hands-on, scenario-driven guide for intermediate Linux learners ready to tackle real-world system administration, DevOps, scripting, and cloud operations.

![Linux](https://img.shields.io/badge/OS-Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![AWS](https://img.shields.io/badge/Cloud-AWS_EC2-FF9900?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Bash](https://img.shields.io/badge/Shell-Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)
![PM2](https://img.shields.io/badge/Process-PM2-2B037A?style=for-the-badge&logo=node.js&logoColor=white)
![systemd](https://img.shields.io/badge/Init-systemd-FFD700?style=for-the-badge)

</div>

---

## 📌 Table of Contents

- [🎯 Goals](#-goals)
- [⚙️ Prerequisites & Environment Setup](#️-prerequisites--environment-setup)
- [📋 Task List (20 Real-World Tasks)](#-task-list-20-real-world-tasks)
  - [🟢 Beginner Tasks (1–5)](#-beginner-tasks-15)
  - [🟡 Intermediate Tasks (6–12)](#-intermediate-tasks-612)
  - [🔴 Advanced Tasks (13–20)](#-advanced-tasks-1320)
- [🌍 Real-World Scenarios Summary](#-real-world-scenarios-summary)
- [🚀 Bonus Challenges](#-bonus-challenges)
- [📖 Quick-Reference Cheatsheet](#-quick-reference-cheatsheet)

---

## 🎯 Goals

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=18&pause=800&color=00FF88&width=700&lines=✅+Take+real+backups+with+tar+%2B+scp;✅+Automate+services+with+systemd;✅+Manage+apps+with+PM2;✅+Schedule+jobs+with+Cron;✅+Master+permissions+%26+user+management)](https://git.io/typing-svg)

By completing this guide you will be able to:

- Back up and transfer files securely between servers using `tar` and `scp`
- Run Docker containers that auto-restart on server reboot using `systemd`
- Manage Node.js processes in production using `PM2`
- Automate scheduled tasks using `cron jobs`
- Confidently manage Linux file permissions, users, and groups
- Write and debug shell scripts for real system administration tasks
- Understand the difference between Amazon Linux and Ubuntu server commands
- Troubleshoot running services, failed processes, and network issues

---

## ⚙️ Prerequisites & Environment Setup

### 🖥️ What You Need

| Requirement | Details |
|---|---|
| OS | Amazon Linux 2 / Ubuntu 20.04+ (EC2 or local VM) |
| Access | SSH key pair for EC2, or local Linux terminal |
| Tools | `tar`, `scp`, `ssh`, `docker`, `systemctl`, `cron`, `pm2` |
| Knowledge | Basic commands: `ls`, `cd`, `mkdir`, `cat`, `chmod`, `nano/vim` |
| Sample Project | [github.com/chetan2624](https://github.com/chetan2624) (public repo) |

### 🔧 Environment Setup

```bash
# 1. SSH into your EC2 instance
ssh -i your-key.pem ec2-user@your-ec2-ip        # Amazon Linux
ssh -i your-key.pem ubuntu@your-ec2-ip          # Ubuntu

# 2. Clone the sample project on EC2
git clone https://github.com/chetan2624/your-repo-name.git
cd your-repo-name

# 3. Update your server (see Task 6 for OS-specific commands)

# 4. Install Docker (Ubuntu)
sudo apt update && sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker

# 5. Install Node.js + PM2 (Ubuntu)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g pm2

# 6. Verify everything is in place
docker --version
pm2 --version
node --version
systemctl status docker
```

---

## 📋 Task List (20 Real-World Tasks)

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=16&pause=700&color=FFD700&width=700&lines=20+Tasks+|+Real+Scenarios+|+Real+Commands;Hands-On+Learning+Like+a+Pro+Engineer)](https://git.io/typing-svg)

---

## 🟢 Beginner Tasks (1–5)

---

### ✅ Task 1 — Backup Code Files from EC2 to Local Using `tar` + `scp`

> **Scenario:** Your code lives on an EC2 server. Your manager asks you to take a local backup before deploying a new version.

**⏱️ Estimated Time:** 20–30 minutes

#### Steps

```bash
# STEP 1: SSH into EC2 and navigate to your project
ssh -i your-key.pem ec2-user@your-ec2-ip
cd /home/ec2-user/

# STEP 2: Archive the project folder using tar
tar -czvf project_backup_$(date +%F).tar.gz your-repo-name/
# -c = create, -z = gzip compress, -v = verbose, -f = filename

# STEP 3: Verify the archive was created
ls -lh project_backup_*.tar.gz

# STEP 4: EXIT EC2 and run scp from your LOCAL machine
exit
scp -i your-key.pem ec2-user@your-ec2-ip:/home/ec2-user/project_backup_*.tar.gz ./backups/

# STEP 5: Verify on local machine
ls -lh ./backups/
tar -tzvf ./backups/project_backup_*.tar.gz   # List contents without extracting
```

#### ✔️ Validation

```bash
# Confirm file exists locally
ls -lh backups/

# Check archive integrity
tar -tzvf backups/project_backup_*.tar.gz | head -20
```

#### ⚠️ Common Pitfalls

- `scp` must be run from **local machine**, not inside EC2
- Always use `$(date +%F)` in filename to avoid overwriting old backups
- Ensure your `.pem` key has correct permissions: `chmod 400 your-key.pem`

---

### ✅ Task 2 — Auto-Restart Docker Container on EC2 Reboot Using `systemd`

> **Scenario:** Your Docker container runs a web app. After an accidental reboot, the container is down and users are affected. Fix it permanently.

**⏱️ Estimated Time:** 30–40 minutes

#### Steps

```bash
# STEP 1: Create a systemd service file
sudo nano /etc/systemd/system/myapp-docker.service
```

Paste this content:

```ini
[Unit]
Description=MyApp Docker Container
After=docker.service
Requires=docker.service

[Service]
Restart=always
RestartSec=10
ExecStartPre=-/usr/bin/docker stop myapp
ExecStartPre=-/usr/bin/docker rm myapp
ExecStart=/usr/bin/docker run --name myapp -p 3000:3000 your-docker-image:latest
ExecStop=/usr/bin/docker stop myapp

[Install]
WantedBy=multi-user.target
```

```bash
# STEP 2: Reload systemd and enable service
sudo systemctl daemon-reload
sudo systemctl enable myapp-docker.service
sudo systemctl start myapp-docker.service

# STEP 3: Verify the service is running
sudo systemctl status myapp-docker.service

# STEP 4: Simulate a reboot
sudo reboot

# STEP 5: After reboot, verify container is running
docker ps
sudo systemctl status myapp-docker.service
```

#### ✔️ Validation

```bash
sudo systemctl is-enabled myapp-docker.service   # Should return: enabled
sudo systemctl is-active myapp-docker.service    # Should return: active
docker ps | grep myapp
```

#### ⚠️ Common Pitfalls

- Forget `daemon-reload` after editing the service file → service won't update
- Use `ExecStartPre=-` (with dash) to ignore errors if container doesn't exist yet
- Make sure Docker itself is enabled: `sudo systemctl enable docker`

---

### ✅ Task 3 — Learn and Use PM2 to Manage Node.js Applications

> **Scenario:** You've deployed a Node.js app on EC2. You need it to stay running after crashes and restarts — without Docker.

**⏱️ Estimated Time:** 30–45 minutes

#### Steps

```bash
# STEP 1: Install PM2 globally
sudo npm install -g pm2

# STEP 2: Start your Node.js app with PM2
cd /home/ec2-user/your-repo-name
pm2 start app.js --name "myapp"

# STEP 3: Explore essential PM2 commands
pm2 list                        # Show all running processes
pm2 logs myapp                  # View real-time logs
pm2 logs myapp --lines 50       # Last 50 log lines
pm2 monit                       # Live monitoring dashboard
pm2 show myapp                  # Detailed process info
pm2 restart myapp               # Restart the app
pm2 stop myapp                  # Stop the app
pm2 delete myapp                # Remove from PM2 list
pm2 reload myapp                # Zero-downtime reload

# STEP 4: Save PM2 process list and enable auto-start on reboot
pm2 save
pm2 startup                     # Follow the command it outputs (copy-paste it)

# STEP 5: Test restart persistence
sudo reboot
# After reboot:
pm2 list                        # App should be running
```

#### ✔️ Validation

```bash
pm2 list                        # Status should be: online
pm2 show myapp | grep status    # Should show: online
curl http://localhost:3000       # App should respond
```

#### ⚠️ Common Pitfalls

- Don't forget `pm2 save` before rebooting — without it, processes won't persist
- `pm2 startup` gives you a command to run — **you must copy and run it manually**
- Use `pm2 logs` first when debugging app crashes

---

### ✅ Task 4 — OS-Specific Commands: Amazon Linux vs Ubuntu

> **Scenario:** You switch between Amazon Linux EC2 and Ubuntu EC2. Commands differ — learn both.

**⏱️ Estimated Time:** 30 minutes

#### Side-by-Side Comparison

| Task | Amazon Linux (yum/dnf) | Ubuntu (apt) |
|---|---|---|
| Update server | `sudo yum update -y` | `sudo apt update && sudo apt upgrade -y` |
| Install package | `sudo yum install -y nginx` | `sudo apt install -y nginx` |
| Remove package | `sudo yum remove nginx` | `sudo apt remove nginx` |
| Search package | `yum search nginx` | `apt search nginx` |
| List installed | `yum list installed` | `dpkg -l` |
| Service start | `sudo systemctl start nginx` | `sudo systemctl start nginx` |
| Check OS version | `cat /etc/os-release` | `lsb_release -a` |

#### Practice Both

```bash
# Amazon Linux
sudo yum update -y
sudo yum install -y git curl wget htop tree
git --version && curl --version

# Ubuntu
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl wget htop tree
git --version && curl --version
```

#### ✔️ Validation

```bash
cat /etc/os-release             # Check which OS you're on
which yum && echo "Amazon Linux"
which apt && echo "Ubuntu"
git --version
```

#### ⚠️ Common Pitfalls

- `apt-get` vs `apt`: both work on Ubuntu; `apt` is newer and recommended
- On Amazon Linux 2023, use `dnf` instead of `yum` (backward compatible)
- Always run `apt update` before `apt install` on Ubuntu

---

### ✅ Task 5 — Automate Backups with Cron Job (tar + cron)

> **Scenario:** Manually taking backups is unreliable. Schedule a daily automatic tar backup using cron.

**⏱️ Estimated Time:** 30–40 minutes

#### Steps

```bash
# STEP 1: Create the backup script
nano /home/ec2-user/scripts/daily_backup.sh
```

```bash
#!/bin/bash
BACKUP_DIR="/home/ec2-user/backups"
PROJECT_DIR="/home/ec2-user/your-repo-name"
DATE=$(date +%F_%H-%M)
LOG_FILE="$BACKUP_DIR/backup.log"

mkdir -p "$BACKUP_DIR"

tar -czvf "$BACKUP_DIR/backup_$DATE.tar.gz" "$PROJECT_DIR" >> "$LOG_FILE" 2>&1

# Keep only last 7 backups
find "$BACKUP_DIR" -name "*.tar.gz" -mtime +7 -delete

echo "[$DATE] Backup completed." >> "$LOG_FILE"
```

```bash
# STEP 2: Make it executable
chmod +x /home/ec2-user/scripts/daily_backup.sh

# STEP 3: Test the script manually first
bash /home/ec2-user/scripts/daily_backup.sh
ls /home/ec2-user/backups/

# STEP 4: Schedule with cron (runs daily at 2 AM)
crontab -e
```

Add this line:

```cron
0 2 * * * /bin/bash /home/ec2-user/scripts/daily_backup.sh
```

```bash
# STEP 5: Verify cron entry
crontab -l

# STEP 6: Check cron service is running
sudo systemctl status cron        # Ubuntu
sudo systemctl status crond       # Amazon Linux
```

#### ✔️ Validation

```bash
crontab -l                        # Cron entry should be visible
ls /home/ec2-user/backups/        # Backup file should exist
cat /home/ec2-user/backups/backup.log
```

#### ⚠️ Common Pitfalls

- Always use **full paths** in cron scripts — cron has a minimal environment
- Test scripts manually before scheduling
- Cron service name is `cron` on Ubuntu and `crond` on Amazon Linux

---

## 🟡 Intermediate Tasks (6–12)

---

### ✅ Task 6 — User & Group Management: Create, Modify, Delete

> **Scenario:** A new developer joins your team. Create a Linux user for them, add them to the right group, and set up their home directory.

**⏱️ Estimated Time:** 25–35 minutes

```bash
# Create a new user with home directory
sudo useradd -m -s /bin/bash devuser

# Set password
sudo passwd devuser

# Create a group
sudo groupadd developers

# Add user to group
sudo usermod -aG developers devuser

# Verify
id devuser
groups devuser
cat /etc/passwd | grep devuser
cat /etc/group | grep developers

# Lock and unlock a user
sudo usermod -L devuser       # Lock
sudo usermod -U devuser       # Unlock

# Delete user (keep home dir)
sudo userdel devuser

# Delete user AND home directory
sudo userdel -r devuser
```

#### ✔️ Validation

```bash
id devuser
getent passwd devuser
ls /home/devuser
```

---

### ✅ Task 7 — File Permission Scenarios (5 Real Challenges)

> **Scenario:** You're managing a shared Linux server. Different users need different levels of access to files and directories.

**⏱️ Estimated Time:** 40–60 minutes

#### 🔐 Challenge 1 — Restrict a Config File to Root Only

```bash
# Only root should read/write this file
sudo chown root:root /etc/myapp/config.conf
sudo chmod 600 /etc/myapp/config.conf
# Verify
ls -l /etc/myapp/config.conf    # Should show: -rw------- root root
```

#### 🔐 Challenge 2 — Shared Folder Where Group Can Write

```bash
# Team folder: owner full access, group can read+write, others nothing
mkdir -p /var/shared/team
sudo chown root:developers /var/shared/team
sudo chmod 770 /var/shared/team
ls -ld /var/shared/team         # Should show: drwxrwx--- root developers
```

#### 🔐 Challenge 3 — Make a Script Executable Only by Owner

```bash
nano /home/devuser/deploy.sh    # Create a script
chmod 700 /home/devuser/deploy.sh
ls -l /home/devuser/deploy.sh   # -rwx------ devuser devuser
```

#### 🔐 Challenge 4 — Sticky Bit on Shared Directory

```bash
# Only file owner can delete their own files (like /tmp)
sudo mkdir /var/shared/uploads
sudo chmod 1777 /var/shared/uploads
ls -ld /var/shared/uploads      # Should show: drwxrwxrwt
```

#### 🔐 Challenge 5 — SetUID on a Script (Run as Owner)

```bash
# Set SUID so the script runs as the file owner, not the caller
sudo chmod u+s /usr/local/bin/myapp-runner
ls -l /usr/local/bin/myapp-runner  # Should show: -rwsr-xr-x
```

#### ✔️ Validation

```bash
stat /etc/myapp/config.conf
stat /var/shared/team
stat /var/shared/uploads
```

---

### ✅ Task 8 — User Permission Scenario: Dev vs Ops Access Control

> **Scenario:** You have developers who should only read logs, and ops users who can modify configs. Set this up correctly.

**⏱️ Estimated Time:** 30–40 minutes

```bash
# Create groups
sudo groupadd devs
sudo groupadd ops

# Create users
sudo useradd -m -G devs alice
sudo useradd -m -G ops bob

# Create directories
sudo mkdir -p /var/app/logs /var/app/config

# Devs can only READ logs
sudo chown root:devs /var/app/logs
sudo chmod 750 /var/app/logs
echo "App started" | sudo tee /var/app/logs/app.log

# Ops can READ and WRITE config
sudo chown root:ops /var/app/config
sudo chmod 770 /var/app/config

# Test as alice (devs group)
sudo -u alice cat /var/app/logs/app.log       # Should succeed
sudo -u alice touch /var/app/config/test.conf # Should FAIL

# Test as bob (ops group)
sudo -u bob touch /var/app/config/app.conf    # Should succeed
sudo -u bob cat /var/app/logs/app.log         # Should FAIL
```

#### ✔️ Validation

```bash
ls -ld /var/app/logs /var/app/config
sudo -u alice ls /var/app/logs
sudo -u bob ls /var/app/config
```

---

### ✅ Task 9 — Find & Fix Broken File Permissions (Audit Task)

> **Scenario:** Someone ran `chmod 777` on production files. Find and fix them.

**⏱️ Estimated Time:** 25 minutes

```bash
# Find all files with 777 permissions
find /var/app -perm 777 -type f

# Find world-writable files
find /var/app -perm -o+w -type f

# Find files with no owner
find / -nouser -type f 2>/dev/null

# Fix permissions in bulk
find /var/app -type f -exec chmod 644 {} \;   # All files: rw-r--r--
find /var/app -type d -exec chmod 755 {} \;   # All dirs: rwxr-xr-x

# Fix ownership
sudo chown -R appuser:appgroup /var/app/
```

#### ✔️ Validation

```bash
find /var/app -perm 777               # Should return nothing
ls -lR /var/app | grep "rwxrwxrwx"   # Should return nothing
```

---

### ✅ Task 10 — Sudo Access Control: Grant Limited sudo Rights

> **Scenario:** A junior dev needs to restart nginx but shouldn't have full sudo. Give them exactly what they need — nothing more.

**⏱️ Estimated Time:** 20–30 minutes

```bash
# Edit sudoers safely
sudo visudo

# Add this line at the bottom:
# devuser ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

```bash
# Test as devuser
sudo -u devuser sudo systemctl restart nginx    # Should work
sudo -u devuser sudo systemctl restart docker   # Should FAIL
sudo -u devuser sudo rm -rf /etc               # Should FAIL

# List a user's sudo privileges
sudo -l -U devuser
```

#### ✔️ Validation

```bash
sudo -l -U devuser                             # Shows allowed commands
sudo -u devuser sudo systemctl status nginx    # Should work
```

---

### ✅ Task 11 — Shell Scripting: Health Check Script

> **Scenario:** Write a script that checks if nginx, docker, and your app are running, and alerts you if anything is down.

**⏱️ Estimated Time:** 40–50 minutes

```bash
nano /home/ec2-user/scripts/health_check.sh
```

```bash
#!/bin/bash
LOG="/home/ec2-user/logs/health.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')
ALERT=0

mkdir -p /home/ec2-user/logs

check_service() {
  SERVICE=$1
  if systemctl is-active --quiet "$SERVICE"; then
    echo "[$DATE] ✅ $SERVICE is running" >> "$LOG"
  else
    echo "[$DATE] ❌ $SERVICE is DOWN! Restarting..." >> "$LOG"
    sudo systemctl restart "$SERVICE"
    ALERT=1
  fi
}

check_port() {
  PORT=$1
  if ss -tulnp | grep -q ":$PORT "; then
    echo "[$DATE] ✅ Port $PORT is open" >> "$LOG"
  else
    echo "[$DATE] ❌ Port $PORT is NOT listening!" >> "$LOG"
    ALERT=1
  fi
}

check_service nginx
check_service docker
check_port 3000
check_port 80

if [ $ALERT -eq 1 ]; then
  echo "[$DATE] ⚠️  Health check found issues. Review log." >> "$LOG"
fi
```

```bash
chmod +x /home/ec2-user/scripts/health_check.sh

# Schedule every 5 minutes
crontab -e
# Add: */5 * * * * /bin/bash /home/ec2-user/scripts/health_check.sh
```

#### ✔️ Validation

```bash
bash /home/ec2-user/scripts/health_check.sh
cat /home/ec2-user/logs/health.log
```

---

### ✅ Task 12 — Disk Usage Monitoring & Cleanup

> **Scenario:** Your server is running low on disk. Identify what's eating space and clean it up safely.

**⏱️ Estimated Time:** 25–35 minutes

```bash
# Check overall disk usage
df -h

# Find the top 10 largest files
du -ah / 2>/dev/null | sort -rh | head -10

# Find files larger than 100MB
find / -type f -size +100M 2>/dev/null

# Find old log files older than 30 days
find /var/log -type f -name "*.log" -mtime +30

# Safe cleanup of old logs
find /var/log -type f -name "*.gz" -mtime +30 -delete

# Clean Docker images and containers
docker system prune -f
docker image prune -a -f

# Clean APT cache (Ubuntu)
sudo apt clean
sudo apt autoremove -y

# Clean yum cache (Amazon Linux)
sudo yum clean all
```

#### ✔️ Validation

```bash
df -h                           # Check disk usage improved
du -sh /var/log                 # Log directory size
docker system df                # Docker space usage
```

---

## 🔴 Advanced Tasks (13–20)

---

### ✅ Task 13 — Process Management & Troubleshooting

> **Scenario:** Your server is slow. Find what process is consuming the most CPU and memory, and kill it safely.

**⏱️ Estimated Time:** 30–40 minutes

```bash
# Real-time process monitoring
top
htop                             # More interactive (install if not available)

# Sort by CPU usage
ps aux --sort=-%cpu | head -10

# Sort by Memory usage
ps aux --sort=-%mem | head -10

# Find a specific process
pgrep -la nginx
pidof nginx

# Check what a process is doing
strace -p <PID>                  # Trace system calls
lsof -p <PID>                    # Open files

# Kill a process gracefully
kill -15 <PID>                   # SIGTERM (graceful)
kill -9 <PID>                    # SIGKILL (force, last resort)

# Kill by name
pkill nginx
killall node
```

#### ✔️ Validation

```bash
ps aux | grep nginx
top -bn1 | head -20
```

---

### ✅ Task 14 — Networking: Check Ports, Connections & Firewall

> **Scenario:** Your app is running but users can't reach it. Debug the network from the inside out.

**⏱️ Estimated Time:** 30–40 minutes

```bash
# Check which ports are listening
ss -tulnp
netstat -tulnp                   # Older systems

# Check if a port is open (from inside)
curl -v http://localhost:3000
wget -q --spider http://localhost:80

# Check from outside using nc
nc -zv your-server-ip 3000

# View active connections
ss -s
ss -t state established

# Firewall status
sudo ufw status                  # Ubuntu
sudo firewall-cmd --list-all     # Amazon Linux

# Open a port
sudo ufw allow 3000/tcp          # Ubuntu
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent  # Amazon Linux
sudo firewall-cmd --reload

# Trace route to a host
traceroute google.com
mtr google.com                   # Better than traceroute

# DNS lookup
nslookup google.com
dig google.com
```

#### ✔️ Validation

```bash
ss -tulnp | grep :3000            # App port should appear
curl -s http://localhost:3000     # Should return app response
sudo ufw status verbose           # Firewall rules visible
```

---

### ✅ Task 15 — SSH Hardening & Key-Based Authentication

> **Scenario:** Secure your EC2 server — disable password auth, change default SSH port, and restrict root login.

**⏱️ Estimated Time:** 35–45 minutes

```bash
# STEP 1: Generate key pair on LOCAL machine
ssh-keygen -t ed25519 -C "your-email@example.com" -f ~/.ssh/myserver_key

# STEP 2: Copy public key to server
ssh-copy-id -i ~/.ssh/myserver_key.pub ec2-user@your-ec2-ip

# STEP 3: Edit SSH config on server
sudo nano /etc/ssh/sshd_config
```

Set these values:

```
Port 2222                        # Change default port
PermitRootLogin no               # Disable root login
PasswordAuthentication no        # Key-only auth
MaxAuthTries 3
AllowUsers ec2-user devuser
```

```bash
# STEP 4: Restart SSH (carefully!)
sudo systemctl restart sshd

# STEP 5: Test from local in NEW terminal (don't close the old one yet!)
ssh -i ~/.ssh/myserver_key -p 2222 ec2-user@your-ec2-ip
```

#### ✔️ Validation

```bash
sudo sshd -t                     # Syntax check before restart
ssh -p 2222 ec2-user@your-ec2-ip # Connect on new port
# Attempt root login — should fail
ssh -p 2222 root@your-ec2-ip
```

#### ⚠️ Common Pitfalls

- Always keep your **current SSH session open** while testing changes
- If using AWS, also open new port in Security Group before changing sshd_config

---

### ✅ Task 16 — Log Analysis & Monitoring

> **Scenario:** Something went wrong overnight. Analyze system logs to find the root cause.

**⏱️ Estimated Time:** 30–40 minutes

```bash
# View system logs
sudo journalctl -xe              # Full systemd journal
sudo journalctl -u nginx         # Specific service logs
sudo journalctl --since "2024-01-01" --until "2024-01-02"
sudo journalctl -f               # Follow live logs

# View traditional log files
sudo tail -f /var/log/syslog     # Ubuntu
sudo tail -f /var/log/messages   # Amazon Linux
sudo cat /var/log/auth.log | grep "Failed password"

# Count failed SSH logins
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn

# Check nginx access logs
sudo tail -100 /var/log/nginx/access.log
sudo tail -100 /var/log/nginx/error.log

# Find ERROR patterns in logs
grep -i "error\|failed\|critical" /var/log/syslog | tail -50

# Monitor multiple logs simultaneously
multitail /var/log/syslog /var/log/nginx/error.log  # Install if needed
```

#### ✔️ Validation

```bash
sudo journalctl -u nginx --since "1 hour ago"
grep "error" /var/log/nginx/error.log | wc -l
```

---

### ✅ Task 17 — Environment Variables & `.env` File Management

> **Scenario:** Your Node.js app needs DB credentials and API keys. Manage them securely on the server.

**⏱️ Estimated Time:** 25–30 minutes

```bash
# Set environment variables temporarily
export DB_HOST="localhost"
export DB_PORT="5432"
echo $DB_HOST

# Make permanent (for current user)
echo 'export DB_HOST="localhost"' >> ~/.bashrc
echo 'export API_KEY="your-key"' >> ~/.bashrc
source ~/.bashrc

# Create a .env file for your app
nano /home/ec2-user/your-repo-name/.env
```

```env
NODE_ENV=production
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=appuser
DB_PASS=supersecret
API_KEY=abc123xyz
```

```bash
# Secure the .env file — only owner can read
chmod 600 .env
chown ec2-user:ec2-user .env

# Add .env to .gitignore (CRITICAL)
echo ".env" >> .gitignore

# Start app with env file using PM2
pm2 start app.js --name myapp --env production
pm2 start app.js --name myapp -- --env-file .env

# View current environment variables
printenv
printenv | grep DB_
env | sort
```

#### ✔️ Validation

```bash
echo $NODE_ENV                    # Should print: production
cat .env | wc -l                  # Non-zero
ls -la .env                       # Should show: -rw------- ec2-user
cat .gitignore | grep .env        # Should be listed
```

---

### ✅ Task 18 — Docker Volume & Network Deep Dive

> **Scenario:** Your Docker container's data is being lost on restart. Learn to use volumes and container networking.

**⏱️ Estimated Time:** 40–50 minutes

```bash
# STEP 1: Run container with a named volume (data persists)
docker volume create myapp_data
docker run -d \
  --name myapp \
  -p 3000:3000 \
  -v myapp_data:/app/data \
  --restart always \
  your-image:latest

# STEP 2: Inspect the volume
docker volume inspect myapp_data
docker volume ls

# STEP 3: Create a custom Docker network
docker network create myapp-net

# STEP 4: Run multiple containers on same network
docker run -d --name db --network myapp-net postgres:14
docker run -d --name app --network myapp-net -e DB_HOST=db your-app:latest

# STEP 5: Containers communicate by service name
docker exec -it app ping db      # Should resolve and ping

# STEP 6: View running containers and logs
docker ps
docker logs -f myapp
docker stats                     # Live resource usage

# STEP 7: Clean up
docker stop myapp && docker rm myapp
docker volume rm myapp_data      # Data gone — intentional
```

#### ✔️ Validation

```bash
docker ps
docker volume ls
docker network ls
docker network inspect myapp-net
```

---

### ✅ Task 19 — Scheduled DB Backup with Cron + Compression + S3 Upload

> **Scenario:** Automate nightly database backups, compress them, and upload to S3 for offsite storage.

**⏱️ Estimated Time:** 50–60 minutes

```bash
# Install AWS CLI
sudo apt install awscli -y          # Ubuntu
sudo yum install awscli -y          # Amazon Linux
aws configure                        # Enter Access Key, Secret, Region

# Create the backup script
nano /home/ec2-user/scripts/db_backup.sh
```

```bash
#!/bin/bash
DATE=$(date +%F_%H-%M)
BACKUP_DIR="/home/ec2-user/db_backups"
DB_NAME="myapp"
DB_USER="postgres"
S3_BUCKET="s3://your-bucket/db-backups"
LOG="$BACKUP_DIR/db_backup.log"

mkdir -p "$BACKUP_DIR"

# Dump the database
pg_dump -U "$DB_USER" "$DB_NAME" | gzip > "$BACKUP_DIR/db_$DATE.sql.gz"

# Upload to S3
aws s3 cp "$BACKUP_DIR/db_$DATE.sql.gz" "$S3_BUCKET/" >> "$LOG" 2>&1

# Delete local backups older than 3 days
find "$BACKUP_DIR" -name "*.sql.gz" -mtime +3 -delete

echo "[$DATE] DB Backup complete and uploaded to S3" >> "$LOG"
```

```bash
chmod +x /home/ec2-user/scripts/db_backup.sh

# Schedule at 3 AM daily
crontab -e
# 0 3 * * * /bin/bash /home/ec2-user/scripts/db_backup.sh
```

#### ✔️ Validation

```bash
bash /home/ec2-user/scripts/db_backup.sh
ls -lh /home/ec2-user/db_backups/
aws s3 ls s3://your-bucket/db-backups/
cat /home/ec2-user/db_backups/db_backup.log
```

---

### ✅ Task 20 — Full Deployment Workflow: Git Pull → Build → PM2 Reload (Zero Downtime)

> **Scenario:** Deploy a new version of your app without any downtime — the professional way.

**⏱️ Estimated Time:** 45–60 minutes

```bash
# Create a deployment script
nano /home/ec2-user/scripts/deploy.sh
```

```bash
#!/bin/bash
set -e                            # Exit immediately on error

APP_DIR="/home/ec2-user/your-repo-name"
BRANCH="main"
LOG="/home/ec2-user/logs/deploy.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

mkdir -p /home/ec2-user/logs

echo "[$DATE] === Deployment Started ===" | tee -a "$LOG"

# STEP 1: Pull latest code
cd "$APP_DIR"
git fetch origin "$BRANCH"
git reset --hard origin/"$BRANCH"
echo "[$DATE] ✅ Code pulled" | tee -a "$LOG"

# STEP 2: Install dependencies
npm install --production
echo "[$DATE] ✅ Dependencies installed" | tee -a "$LOG"

# STEP 3: Run tests (optional)
# npm test

# STEP 4: Reload PM2 with zero downtime
pm2 reload myapp --update-env
echo "[$DATE] ✅ PM2 reloaded — Zero downtime deploy complete!" | tee -a "$LOG"

# STEP 5: Health check
sleep 5
HTTP_CODE=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:3000)
if [ "$HTTP_CODE" == "200" ]; then
  echo "[$DATE] ✅ Health check passed (HTTP 200)" | tee -a "$LOG"
else
  echo "[$DATE] ❌ Health check FAILED (HTTP $HTTP_CODE)" | tee -a "$LOG"
  pm2 reload myapp                # Retry
fi

echo "[$DATE] === Deployment Complete ===" | tee -a "$LOG"
```

```bash
chmod +x /home/ec2-user/scripts/deploy.sh

# Run the deployment
bash /home/ec2-user/scripts/deploy.sh

# Watch logs during deploy
pm2 logs myapp --lines 20
cat /home/ec2-user/logs/deploy.log
```

#### ✔️ Validation

```bash
curl http://localhost:3000         # App should respond
pm2 list                           # Status: online
cat /home/ec2-user/logs/deploy.log # Deployment steps logged
git log --oneline -5               # Latest commits pulled
```

---

## 🌍 Real-World Scenarios Summary

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=16&pause=700&color=FF6B6B&width=700&lines=System+Admin+|+DevOps+|+Cloud+|+Security+|+Scripting)](https://git.io/typing-svg)

| # | Scenario | Tasks Covered | Category |
|---|---|---|---|
| 1 | Code backup from EC2 to local | 1, 5 | Backup & Storage |
| 2 | Docker auto-restart on reboot | 2, 18 | DevOps & Docker |
| 3 | Node.js app lifecycle management | 3, 20 | PM2 & Deployment |
| 4 | Cross-distro server management | 4 | System Admin |
| 5 | Scheduled automated backups | 5, 19 | Automation & Cron |
| 6 | Team access control setup | 6, 7, 8, 10 | Security & Permissions |
| 7 | Production incident debugging | 9, 13, 16 | Troubleshooting |
| 8 | Network connectivity issues | 14, 15 | Networking |
| 9 | Secret management | 17 | Security |
| 10 | Zero-downtime deployments | 20 | DevOps & CI/CD |

---

## 🚀 Bonus Challenges

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=16&pause=700&color=B388FF&width=700&lines=Push+your+limits+with+these+bonus+challenges!)](https://git.io/typing-svg)

### 🔥 Bonus 1 — Nginx Reverse Proxy Setup
Configure Nginx to proxy `http://your-domain.com` → `http://localhost:3000` with caching.

### 🔥 Bonus 2 — SSL/HTTPS with Let's Encrypt
Install Certbot and get a free SSL certificate for your domain.
```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com
```

### 🔥 Bonus 3 — Docker Compose for Full Stack
Write a `docker-compose.yml` that runs your app + PostgreSQL + Redis together.

### 🔥 Bonus 4 — Fail2Ban for SSH Security
Install and configure Fail2Ban to auto-block IPs after 3 failed SSH attempts.
```bash
sudo apt install fail2ban
sudo systemctl enable fail2ban
```

### 🔥 Bonus 5 — Monitor Server Metrics to Slack/Discord
Write a cron script that sends CPU/RAM/Disk alerts to a Slack webhook when thresholds are exceeded.

### 🔥 Bonus 6 — Create a Full Bash Menu-Driven Admin Tool
Build a shell script with a numbered menu: `[1] Backup [2] Deploy [3] Health Check [4] Logs` etc.

### 🔥 Bonus 7 — Automate EC2 Snapshots using AWS CLI
Write a cron job that creates an EBS snapshot of your EC2 volume and deletes snapshots older than 7 days.

---

## 📖 Quick-Reference Cheatsheet

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=16&pause=700&color=00CFFF&width=700&lines=Your+Linux+Command+Pocket+Guide+🚀)](https://git.io/typing-svg)

### 📦 tar Commands

```bash
tar -czvf archive.tar.gz folder/      # Create compressed archive
tar -xzvf archive.tar.gz              # Extract archive
tar -tzvf archive.tar.gz              # List contents (no extract)
tar -czvf backup_$(date +%F).tar.gz . # Date-stamped archive
```

### 🔁 scp Commands

```bash
scp -i key.pem file.txt user@ip:/remote/path/    # Upload to server
scp -i key.pem user@ip:/remote/file.txt ./local/ # Download from server
scp -i key.pem -r folder/ user@ip:/remote/path/  # Upload folder recursively
```

### 🐳 Docker Commands

```bash
docker ps                          # Running containers
docker ps -a                       # All containers
docker images                      # List images
docker logs -f container_name      # Follow logs
docker exec -it container bash     # Shell into container
docker stop / start / restart name # Manage containers
docker system prune -f             # Clean unused resources
docker stats                       # Live resource usage
```

### ⚙️ systemd Commands

```bash
sudo systemctl start service       # Start
sudo systemctl stop service        # Stop
sudo systemctl restart service     # Restart
sudo systemctl enable service      # Start on boot
sudo systemctl disable service     # Don't start on boot
sudo systemctl status service      # Check status
sudo systemctl daemon-reload       # After editing .service file
sudo journalctl -u service -f      # Follow service logs
```

### 🔄 PM2 Commands

```bash
pm2 start app.js --name myapp     # Start app
pm2 list                          # List all apps
pm2 logs myapp                    # View logs
pm2 restart myapp                 # Restart
pm2 reload myapp                  # Zero-downtime reload
pm2 stop myapp                    # Stop
pm2 delete myapp                  # Remove from list
pm2 save                          # Save current list
pm2 startup                       # Enable auto-start on boot
pm2 monit                         # Live monitor
```

### 🔐 Permissions Cheatsheet

```bash
chmod 777 file    # rwxrwxrwx — everyone full access (AVOID)
chmod 755 file    # rwxr-xr-x — owner all, others read+exec
chmod 644 file    # rw-r--r-- — owner read+write, others read
chmod 600 file    # rw------- — owner only (secrets, keys)
chmod 700 dir     # rwx------ — owner only
chmod +x file     # Add execute permission
chmod -x file     # Remove execute
chown user:group file             # Change owner and group
chown -R user:group folder/       # Recursive ownership change
find / -perm 777 -type f          # Find world-writable files
```

### 👥 User Management

```bash
useradd -m -s /bin/bash username  # Create user with home
passwd username                   # Set password
usermod -aG groupname username    # Add to group
userdel -r username               # Delete user + home
id username                       # Show user/group info
groups username                   # List user's groups
cat /etc/passwd                   # All users
cat /etc/group                    # All groups
sudo visudo                       # Edit sudoers safely
```

### 📅 Cron Syntax

```
*  *  *  *  *  command
│  │  │  │  └── Day of week (0=Sun, 6=Sat)
│  │  │  └───── Month (1–12)
│  │  └──────── Day of month (1–31)
│  └─────────── Hour (0–23)
└────────────── Minute (0–59)
```

```bash
crontab -e                        # Edit cron jobs
crontab -l                        # List cron jobs
crontab -r                        # Remove all cron jobs

# Examples
0 2 * * *  /bin/bash /scripts/backup.sh     # Daily at 2 AM
*/5 * * * * /bin/bash /scripts/health.sh    # Every 5 minutes
0 0 * * 0  /bin/bash /scripts/weekly.sh    # Every Sunday midnight
```

### 🌐 Networking

```bash
ss -tulnp                         # Show listening ports
curl -v http://localhost:3000     # Test HTTP endpoint
ping google.com                   # Test connectivity
nslookup domain.com               # DNS lookup
dig domain.com                    # Detailed DNS query
traceroute domain.com             # Network path
sudo ufw status                   # Firewall status (Ubuntu)
sudo ufw allow 3000/tcp           # Open port (Ubuntu)
```

### 📊 Process & System

```bash
top                               # Real-time processes
htop                              # Interactive process viewer
ps aux --sort=-%cpu | head -10    # Top CPU consumers
ps aux --sort=-%mem | head -10    # Top Memory consumers
df -h                             # Disk usage
du -sh folder/                    # Folder size
free -h                           # RAM usage
uptime                            # Server uptime + load
kill -15 PID                      # Graceful kill
kill -9 PID                       # Force kill
```

---

<div align="center">

[![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=20&pause=1000&color=00FF88&center=true&vCenter=true&width=700&lines=Keep+Building.+Keep+Breaking.+Keep+Learning.+🐧;Linux+Mastery+is+a+Journey+—+Not+a+Destination!)](https://git.io/typing-svg)

**Made with ❤️ for Linux learners who prefer doing over reading.**

![Tasks](https://img.shields.io/badge/Tasks-20_Completed-00C853?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Intermediate_to_Advanced-FF6D00?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-EC2_%7C_Ubuntu_%7C_Amazon_Linux-232F3E?style=for-the-badge&logo=amazon-aws)

</div>
