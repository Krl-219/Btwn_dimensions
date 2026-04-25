#Linux #opensource

# Linux Distro Cheat Sheet: RHEL/SUSE vs. Ubuntu

**Target Audience:** System Administrators transitioning between Enterprise (RHEL/SUSE) and Debian-based (Ubuntu) environments. **Date:** April 2026

## 1. Package Management

_The most frequent point of friction._

|Action|RHEL / SUSE (RPM-based)|Ubuntu (DEB-based)|Notes|
|---|---|---|---|
|**Install Package**|`dnf install <pkg>`|`apt install <pkg>`|`yum` is legacy on RHEL 7/8; `dnf` is standard on 8/9+.|
|**Remove Package**|`dnf remove <pkg>`|`apt remove <pkg>`|`purge`removes config files too (`apt purge`).|
|**Update System**|`dnf update`|`apt update && apt upgrade`|Ubuntu requires two steps: refresh list, then upgrade.|
|**Search Package**|`dnf search <keyword>`|`apt search <keyword>`||
|**Show Info**|`dnf info <pkg>`|`apt show <pkg>`||
|**List Installed**|`dnf list installed`|`dpkg --list` or `apt list --installed`||
|**Add Repository**|Edit `/etc/yum.repos.d/*.repo`|Edit `/etc/apt/sources.list` or `/etc/apt/sources.list.d/`||
|**Refresh Repos**|`dnf clean all && dnf makecache`|`apt update`||

## 2. Service Management (Systemd)

_Commands are identical, but service **names** often differ._

|Action|Command (Universal)|RHEL/SUSE Service Name|Ubuntu Service Name|
|---|---|---|---|
|**Start Service**|`systemctl start <name>`|`httpd`|`apache2`|
|**Stop Service**|`systemctl stop <name>`|`httpd`|`apache2`|
|**Restart Service**|`systemctl restart <name>`|`httpd`|`apache2`|
|**Enable on Boot**|`systemctl enable <name>`|`httpd`|`apache2`|
|**Check Status**|`systemctl status <name>`|`httpd`|`apache2`|
|**View Logs**|`journalctl -u <name> -f`|`httpd`|`apache2`|
|**Web Server**|N/A|`httpd`|`nginx` (same name)|
|**Database (MySQL)**|N/A|`mysqld`|`mysql`|
|**Database (Postgres)**|N/A|`postgresql`|`postgresql`|

## 3. Networking

_Configuration syntax and default tools differ significantly._

|Task|RHEL / SUSE|Ubuntu|
|---|---|---|
|**IP Address**|`ip addr` (Same)|`ip addr` (Same)|
|**Connectivity**|`ping`, `curl`, `wget` (Same)|`ping`, `curl`, `wget`(Same)|
|**Firewall (Default)**|`firewalld`|`ufw` (Uncomplicated Firewall)|
|**Firewall Command**|`firewall-cmd --add-port=80/tcp --permanent`|`ufw allow 80/tcp`|
|**Reload Firewall**|`firewall-cmd --reload`|`ufw reload`|
|**Network Config**|`/etc/sysconfig/network-scripts/ifcfg-eth0`(Legacy)  <br>`nmcli` (Modern)|`/etc/netplan/01-netcfg.yaml`(Modern)  <br>`/etc/network/interfaces` (Legacy)|
|**Apply Netplan**|N/A|`sudo netplan apply`|
|**DNS Config**|`/etc/resolv.conf`(Managed by NetworkManager)|`/etc/resolv.conf`(Managed by systemd-resolved)|

## 4. Security & Access Control

_Critical difference: SELinux vs. AppArmor._

|Feature|RHEL / SUSE|Ubuntu|
|---|---|---|
|**Mandatory Access**|**SELinux** (Enabled by default)|**AppArmor** (Enabled by default)|
|**Status Check**|`getenforce`|`aa-status`|
|**Disable Temporarily**|`setenforce 0`|`apparmor_parser -R /etc/apparmor.d/<profile>`|
|**Logs Location**|`/var/log/audit/audit.log`|`/var/log/kern.log` or `dmesg`|
|**Troubleshooting**|`ausearch`, `semanage`|`aa-logprof`, `apparmor_status`|
|**File Permissions**|`chmod`, `chown`(Same)|`chmod`, `chown` (Same)|

## 5. Common Configuration Files

_Paths are mostly standard, but specific file names vary._

|Component|RHEL / SUSE Path|Ubuntu Path|
|---|---|---|
|**Web Server Config**|`/etc/httpd/conf/httpd.conf`|`/etc/apache2/apache2.conf`|
|**Virtual Hosts**|`/etc/httpd/conf.d/`|`/etc/apache2/sites-available/`|
|**Nginx Config**|`/etc/nginx/nginx.conf`|`/etc/nginx/nginx.conf`|
|**Nginx Sites**|`/etc/nginx/conf.d/`|`/etc/nginx/sites-enabled/`|
|**Cron Jobs**|`/etc/crontab`, `/etc/cron.d/`|`/etc/crontab`, `/etc/cron.d/`|
|**User Shell**|`/etc/shells`|`/etc/shells`|
|**Hostname**|`/etc/hostname`|`/etc/hostname`|
|**Hosts File**|`/etc/hosts`|`/etc/hosts`|

## 6. Quick Translation Guide (Mental Map)

- **If you type `yum` or `dnf`...**
    - Switch to `apt` on Ubuntu.
- **If you look for `httpd`...**
    - Look for `apache2` on Ubuntu.
- **If you edit `firewalld` rules...**
    - Switch to `ufw` on Ubuntu.
- **If you struggle with `SELinux` denials...**
    - Check `AppArmor` logs (`dmesg` or `aa-status`) on Ubuntu.
- **If you edit network scripts in `/etc/sysconfig/`...**
    - Edit YAML files in `/etc/netplan/` on Ubuntu.

## 7. Emergency Troubleshooting Steps

_(Works on all three)_

1. **Check Disk Space:** `df -h`
2. **Check Memory:** `free -m`
3. **Check Running Processes:** `top` or `htop`
4. **Check Listening Ports:** `ss -tulpn` (Preferred) or `netstat -tulpn`
5. **Check Logs:** `journalctl -xe` (Systemd logs)
6. **Test Connectivity:** `ping <host>`, `telnet <host> <port>`, `nc -zv <host> <port`