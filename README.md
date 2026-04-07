![NextDNS DDNS Updater Logo](nextdns-ddns-updater-logo.png.png)
# NextDNS DDNS Updater (Linux)

![Linux](https://img.shields.io/badge/platform-linux-blue)
![Bash](https://img.shields.io/badge/script-bash-green)
![Docker](https://img.shields.io/badge/docker-supported-blue)
![License](https://img.shields.io/badge/license-MIT-yellow)
![Status](https://img.shields.io/badge/status-stable-success)

A lightweight Bash-based Dynamic DNS updater for NextDNS that automatically updates your Linked IP when your public IP changes.

Designed for Linux servers, VPS environments, and homelabs.

---

# Features

* Automatic NextDNS IP updates
* Works on most Linux distributions
* Cron automation support
* systemd timer support
* Docker container support
* Timestamped logging
* Log rotation support
* Lightweight (bash + curl)

---

# Requirements

* Linux server (Ubuntu / Debian recommended)
* `curl`
* `cron` or `systemd`
* NextDNS account

Compatible with:

* Ubuntu Server
* Debian
* VPS environments
* Homelab servers
* Proxmox hosts LXC/VMs

Hardware:
> Recommended hardware requirements:
* Ram 1GB
* CPU: 1 Core
* Disk Size: 8GB

---

# Architecture

This project keeps the **NextDNS Linked IP updated automatically**.

Flow of operation:

![Flow of operation](Flowofoperation.png)

```
https://link-ip.nextdns.io/YOUR_TOKEN
```

▼
NextDNS Linked IP Updated

This ensures your NextDNS configuration always sees the correct public IP.

---

# Installation

## 1 Clone the Repository

```
git clone https://github.com/sami-alrustom/nextdns-ddns-updater.git
cd nextdns-ddns-updater
```

---

## 2 Configure Your NextDNS Link

Edit the script and replace the token with your NextDNS update link.

```
nano update-nextdns.sh
```

Update this line:

```
URL="https://link-ip.nextdns.io/YOUR_TOKEN"
```

---

# Cron Setup (Simple Method)

Install the script:

```
sudo cp update-nextdns.sh /usr/local/bin/
sudo chmod +x /usr/local/bin/update-nextdns.sh
```

Create log file:

```
sudo touch /var/log/nextdns-ddns.log
sudo chmod 600 /var/log/nextdns-ddns.log
```

Edit cron:

```
sudo crontab -e
```

Add:

```
*/5 * * * * /usr/local/bin/update-nextdns.sh
```

This will update your NextDNS IP every **5 minutes**.

---

# systemd Timer (Recommended Modern Method)

Copy the service files:

```
sudo cp systemd/nextdns-ddns.service /etc/systemd/system/
sudo cp systemd/nextdns-ddns.timer /etc/systemd/system/
```

Reload systemd:

```
sudo systemctl daemon-reload
```

Enable timer:

```
sudo systemctl enable --now nextdns-ddns.timer
```

Check timers:

```
systemctl list-timers
```

---

# Docker Version

You can also run the updater in Docker.

## Build container

```
docker build -t nextdns-ddns ./docker
```

## Run container

```
docker run -d \
--name nextdns-ddns \
nextdns-ddns
```

The container will run the updater every **5 minutes**.

---

# Logging

Every update attempt is logged.

Log file location:

```
/var/log/nextdns-ddns.log
```

Example output:

```
root@nextDNS-DDNS:~# tail -n 5 /var/log/nextdns-ddns.log
2026-03-12 18:25:01 [SUCCESS] IP Updated: 65.254.64.177
2026-03-12 18:30:02 [SUCCESS] IP Updated: 65.254.64.177
2026-03-12 18:35:01 [SUCCESS] IP Updated: 65.254.64.177
2026-03-12 18:40:01 [SUCCESS] IP Updated: 65.254.64.177
2026-03-12 18:45:01 [SUCCESS] IP Updated: 65.254.64.177

```

---

# Log Rotation

To prevent the log file from growing indefinitely, install logrotate configuration:

```
sudo cp logrotate-nextdns-ddns /etc/logrotate.d/nextdns-ddns
```

Log rotation settings:

* Weekly rotation
* Keep last 4 logs
* Compress old logs

---

# Testing

Run the script manually:

```
sudo /usr/local/bin/update-nextdns.sh
```

Check logs:

```
tail -f /var/log/nextdns-ddns.log
```

Verify in your NextDNS dashboard that your **Linked IP** has updated.

---

# Author

**Sami Alrustom**

Network Technician Student
Homelab & Networking Enthusiast

---

# License

This project is licensed under the **MIT License**.
