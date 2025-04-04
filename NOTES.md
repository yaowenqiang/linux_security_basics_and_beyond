# Linux Security - Basics and Beyond

> https://github.com/daveprowse/linux-security


## Linux User Security


User Types:

+ Superuser account
+ Regular accounts

### Principle of Least Privilege

Users and processes should only receive the Privilege that are vital for their work


> id

### Working with Passwords
 

> 16 - 20 characters

> NIST SP 800-63B Digital Identity Guidelines

> passwd


### Generating Passwords with openssl and KeePass

> ssh -V
> openssl passwd -6
> openssl rand -base64 24
> openssl rand -hex 24
> openssl rand -hex -out pass.txt 24

### Passwd and shadow

> /etc/passwd
> /etc/shadow

### Password settings

> adduser sysadmin
> chage sysadmin

> /etc/login.defs

> chage -d 0 sysadmin # force sysadmin to change password at next login


### Password Policy

> Pluggable authentication modules (PAM)

/etc/pam.d/common-password

> password        [success=1 default=ignore]      pam_unix.so obscure yescrypt minlen=12

> apt install libpam-pwquality

> password pam_pwquality.so retry=3 ucredit=1 dcredit=1 ocredit=-2 minclass=3 

### Linux authentication


> usb key authentication

> set shortcut key to logut

> gnome-session-quit --logout --no-prompt
> alias out="gnome-session-quit --logout --no-prompt"

> Ctrl + Alt + F1
> Ctrl + Alt + F2
> Ctrl + Alt + F4

### Enterprise login

> FreeIPA

> dnf install freeipa-client
> nmcli c m 'connection mame' ipv4.dns dnsip
> nmcli c u 'connection mame'
> nmcli
> hostnamectl

> ipa-client-install --enable-dns-updates --makehomedir

### Locking the System

### SSH basics

> ss -ant
> ss -tun

> watch  ss -tp


### SSH and linux in the Cloud

> set ssh key  permission to 400
> chmod 400 filename

> systemctl status

> terraform status

> cat /etc/debian-version

## su, sudo,and sudoers

### su

> su is short for 'substitute user'

> su <username> Switches to that user account but does not change the path or environment.(it doens't open an actural new login shell associate with that user.) 
> su - <username> Switches to that user account withing the new  user's home directory.Has full access to the user's environment

The username can be ommitted when switching to root

### sudo

+ a program
+ a group(sudo/ wheel)
+ a command


sudo <command> Runs a command with sudo privileges(if the user is part of sudoers)(superuser do!)
sudo -i Gives access to root account (if the user has full sudo privileges), Similar to su -,Also, accesses the root account in Ubuntu and Fedora (where the root accout has not password by default)

> apt install sudo


### sudoers

> /etc/sudoers

> visudo
> Defaults user timestamp_timeout=30 

### Assigning a Regular User sudo Permissions

> usermod -aG wheel sysadmin
> usermod -aG sudo sysadmin

## Linux Operating System Security

+ OS Updates & Upgrades
+ User Education/Awareness
+ Trusting Nothing

### Defence in Depth

+ network
  + snort
  + suricata
+ host
+ application
+ data

### The CIA Triad

CIA Triad（机密性、完整性、可用性）是信息安全的三大核心原则，旨在为数据保护提供系统性框架

### Updating Linux

#### debian

apt
apt update
apt list --upgradable
apt upgrade
apt install <package(s)>

> sudo apt install && apt upgrade -y

> dpkg -i package_file.deb


> apt clean
> apt autoclean

> apt full-upgrade

> /etc/apt/sources.list.d/

> apt edit-sources

> add-apt-repository


### Fedora, Red Hat, and CentOS


.rpm
dnf
dnf update
dnf update --downloadonly
dnf check-update
dnf update or dnf install <packagename>

### Installing Security Updates only

> dnf check-update --security
> dnf update --security

> https://access.redhat.com/security/security-updates/

> dnf update --advisory=

> apt-get -s dist-upgrade | grep '^inst' | grep -i security
> apt-get -s dist-upgrade | grep '^inst' | grep -i security | awk -F" " '{print $2}' | xargs apt-get install


> unattended-upgrades
> debescan
> anacron

### Upgrading SUSE


.rpm
.repo
YaST2
zypper

> zypper refresh
> zypper install 
> zypper update 


### Upgrading Arch


.pkg
.tar

pacman -Syu
pacman -Syyu

### Working with Services and System Health

#### systemctl basics


unit file commands

systemctl
    enable
    disable
    list-unit-files

unit commands


systemctl
    start
    stop
    restart
    list-units

#### Reducing the Attack Surface

> systemctl --now disable networking

> systemctl list-unit-files  --type service | grep enabled

> ss -tulnw

>apt purge

> systemctl --failed
> journalctl -u service-name

### systemd states(unit)

| State | Description |
| -------------- | --------------- |
| Active | The service has been successfully run by systemd |
| Inactive | The service was not activated during boot or manually |
| Activating | The service is in the process of becoming active |
| Deactivating | The service is in the process of becoming inactive |
| Failed | The service failed to activate |
| Not-found | The service is not found(does not exist in Linux)|
| dead | The service is dead(stpped)|


> systemctl reload
> systemctl daemon-reload


### systemd states(unit files)

| State | Description |
| -------------- | --------------- |
| Enabled | Will attempt to start up every time the system starts |
| Disabled | Will not attempt to start up every time the system starts |
| Masked | completely disabled, no start operation will work unless it is unmasked |
| Linked | Made avaiable through one or more symlinks |
| Static | Unit file is not enabled, and has no provisions to be enabled |
| Transient | Unit file was created dynamically and cannot be enabled |
| generated | Created dynamically with a generator tool(systemd generator) |
| Bad | Unit file is invalid or another error occured |

### Securing Linux Distros

#### 10 Steps to a Secure Linux Server

step 1 - hardware and BIOS Set a complex password!
step 2 - Installation Considerations, Minimum amount of software
step 3 - Post installation, Updates, security mailing list
step 4 - Harden the system, Remove unnecessary services
step 5 - Secure Networking File transfers, Network Access
step 6 - Secure the Users Password Security
step 7 - Use and Secure SSH, Disable root! Use keys
step 8 - Set up a Firewall, firewalld, nftables, ufw, etc...
step 9 - Set up IDS/IPS, snort, suricata, etc...
step 10 - Backup and auditing, Built-in tools(rsync), cp, scp, dd, zfs send, zfs receive, or third-party tools, such as borg, or Deja Dup, or Duplicati, or Syncthing


> check file system integrity with tools such as AIDE and Tripwire or debsums
> protect applications with Linux Security modules like AppArmor and SELinux
> use tools such fail2ban to block malicious requests, or clamAV for malware or rkhunter to check for root kits
> don't send ICMP rediects and ignore ICMP broadcasts
> enable TCP synchronization cookies
> check IP Scheme and verify that it works withing your environment
> Watch for bad DNS forwarding, close open ports on your main system and your firewall
> consider masking your IP address
> create non-root systems administrators

> ubuntu live patch  - upgrade kernel without reboot

### Wired and Wireless Security on Linux

#### Block Protocols at the Kernel Level


>for debian systems

> touch /etc/network/secure-interface-ens3


```bash
#!/bin/bash

echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
echo 0 > /proc/sys/net/ipv4/conf/all/forwarding


```

> chmod +x secure-interface-ens3


add pre-up to interface file

> pre-up /etc/network/secure-interface-ens3


> reboot the server


> cd /sys/class/net/eth0/statisc/
> cat rx_packets
> cat tx_packets
> cat collisions
> cat tx_errors
> cat xx_errors


### Securing GRUB

> grub> vbeinfo
> set pager=1

> cd /etc/grub.d/
> vim 40_custom
> set superusers="user"

> pbkdf2 

> grub-mkpasswd-pbkdf2
> password_pbkdf2 user hashd-password
> update-grub

> vim /etc/grub.d/10_linux
> find CLASS line 
> add  --unrestricted
> update-grub

### Application Security

#### AppArmor Basics

> apt list | grep 'apparmor'

> apt policy apparmor
> systemctl status apparmor
> aa-enabled
> aa-status
> systemctl --now disable apparmor

> aa-teardown  # disable apparmor temporarily

### Apparmor profiles

> apt install apparmor-utils auditd

> aa-genprof human
> /etc/apparmor.d/usr.local.bin.human

> aa-complain usr.local.bin.human
> aa-enforce usr.local.bin.human
> aa-disable usr.local.bin.human

> rm /etc/apparmor.d/usr.local.bin.human














