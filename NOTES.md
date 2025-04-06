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


#### AppArmor and Apache Example

> apt install apache2
> apt install libapache2-mod-apparmor
> aa-enforce usr.sbin.apache2

> lateral attacks

### SeLinux Basics

> sestatus

> getenforce


```bash
sudo vi /etc/selinux/config
SELINUX=enforcing   # 强制模式（推荐生产环境）
reboot

```

> semanage port -a -t ssh_port_t -p tcp 2222 


> firewall-cli --list ports
> firewall-cli --list services
> firewall-cli --add-port=2222/tcp  --permanet
> firewall-cli --reload









## Firewall and SSH Security

### What is a Firewall

+ Firewalls permit or deny traffic
+ Can be hardware or software
+ Protect

### Types of Linux-based Firewalls

+ UFW
  + Uncomplicated Firewall
  + Developed by Canonical
  + Easy to use
  + Limited security
+ firewalld
  + Fairly comprehensive firewall
  + Developed by Red Hat
  + Designed for workstation and servers
  + Front-end for nftables
+ nftables
  + Today's sophisticated backend firewall
  + Works directly with the linux kernel
  + Utilizes the nft front-end tool
  + Designed for networks and servers
+ IPFire
+ the Endian Firewall Community
+ EFW
+ GUFW
+ OpenWrt for routers
+ ClearOS
+ freebsd pfSense and OPNsense


### Zero-trust mindset

Trust Nothing
Authenticate Everything

> netcat -v ip port




### UFW

> apt install ufw

| Command | Description |
| -------------- | --------------- |
| ufw status | show weather UFW is active or inactive |
| ufw enable | starts and enables the firewall |
| ufw show added | lists UFW rules |
| ufw reset | disable UFW and deletes rules |

#### Default global rules:

> ufw default deny incoming
> ufw default allow outgoing

#### Setting up UFW

> apt install ufw
> systemctl enable --now ufw
> ufw enable
> apt install nmap
> nmap ip
> nmap ip -Pn

> vim /etc/default/ufw
> IPV4=no
> vim /etc/ufw/user.rules

> ufw allow ssh
> ufw allow 22/tcp
> ufw show added
> ufw deny ssh

> ufw status numbered
> ufw delete 1
> ufw allow proto tcp to 0.0.0.0/24 port 22

> ufw reload
> ufw reset

### firewalld


| Command | Description |
| -------------- | --------------- |
| systemctl status firewalld | Check the status of the services |
| firewall-cmd --list-ports | Displays open networking ports(if any) |
| firewall-cmd --add-port=80/tcp | Open port 80 on the system |
| firewall-cmd --get-active-zones | Show the type of zone in use by each network interface |


> yum install firewalld

> firewall-cmd --get-active-zones
> firewall-cmd --list-all
> firewall-cmd --get-zones
> firewall-cmd --list-all-zones


> firewall-cmd --zone=block --change-interface=eth0 --permanet
> firewall-cmd --zone=block --list-all
> firewall-cmd --zone=block --add-port=22/tcp --perm
> firewall-cmd --zone=block --remove-port=22/tcp --perm
> firewall-cmd --get-default-zone

> firewall-cmd --reload

> firewall-cmd --set-default-zone block

#### open multiple ports

> firewall-cmd --zone=block --add-port={22,443,389, 466,88, 464}/tcp --perm


### lock it down

> firewall-cmd --panic-on # isolate on layer three
> firewall-cmd --panic-off


> man firewalld
> man firewall-cmd


### nftables

> NetFilter tables

### Securing SSH

#### ip_tables

include four kernel modules

+ ip_tables
+ ip6_tables
+ arp_tables
+ ebtales


nftables 


> first release was in 2014

> apt install nftables

> nft list ruleset

> nft -i
> nft> add table inet ports_table
> nft> list ruleset
> nft> add chain inet ports_table input {type filter hook input priority 00; policy drop; }
> nft> add rule inet ports_table input tcp dport 22 accept
> nft> add rule inet ports_table input icmp type { echo-request } accept
> nft> add rule inet ports_table input icmp type { echo-reply } accept
> nft> add rule inet ports_table input tcp sport 53 accept # dns request
> nft> add rule inet ports_table input udp sport 53 accept # dns request

> nft -a list table inet ports_table
> nft delete rule inet ports_table input handle 5
> nft delete chain inet ports_table  handle 1 # delete chain
> nft flush table <table_name>
> nft flush ruleset # remove everything

### Save and Restoring the nftables Configuration

> nft flush ruleset # remove everything

>  /etc/nftables.conf on debian or ubuntu
>  /etc/sysconfig/nftables.conf on centos
> /etc/nftables/main.nft  templates

### Translate iptables to nftables

> apt install iptables

> iptables-translate --version
> iptables-save

> iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT
> iptables-translate -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT


> sudo nft add table inet filter 
> sudo nft add chain inet filter input { type filter hook input priority 0 ; }
> nft add rule inet filter input tcp dport 22 ct state new counter accept

## Securing SSH


### Create a strong RSA key pair

> ssh-keygen -b 4096
> ssh-keygen -b 8192
> ssh-keygen -t ed25519
> ssh-copy-id -i "id_ed25519.pub" user@ip
> ssh -i "id_ed25519" user@ip

> man RSA
> man ed25519

> vim /root/.ssh/config

```
Host myhost
    HostName myhostname
    IdentityFile ~/.ssh/id_ed25519
    User user
```


### The sshd_config file


> /etc/ssh/
> /etc/ssh/sshd_config

> backup before any changes

#### Modifying the default SSH port

> port 2222

#### Disable Password-based SSH

> PasswordAuthentication no

### Disable root login via SSH

> PermitRootLogin prohibit-password  # can't login as root via password,but can login via key

> PermitRootLogin no

#### Exclusive SSH Groups

> addgroup ssh-allowed
> adduser user -g ssh-allowed
> usermod -g ssh-allowed user
> groups user

> AllowGroups ssh-allowed

> deluser user ssh-allowed # debian

> userdel user ssh-allowed # centos

> sudo usermod -g users user
> sudo usermod -d user ssh-allowed

#### Authentication Settings

> MaxAuthTries 3
> LoginGraceTime 15s # time to wait for type password 

> X11Forwarding no # server side

> /etc/ssh/ssh_config

> ForwardX11 no # client side
> ForwardX11Trusted no # client side
> PasswordAuthentication no # client side

> ssh -X/-Y # x11 forwarding

> man sshd_config

### Terminating SSH Connections

> pgrep -c sshd

> ps -ef | grep ssh

> ss -punt
> watch ss -punt

> kill
> pkill --signal KILL sshd
> apt install psmisc
> killall "sshd"


### auto logout after certain time of inactivity

> vim /home/user/.bash_profile
```bash
TMOUT=180
readonly TMOUT
export TMOUT
```

> vim /etc/profile.d/custom.sh
> vim /etc/profile

```bash
#!/bin/bash
TMOUT=180
readonly TMOUT
export TMOUT
```
#### SSH Key management

> https://www.ssh.com/academy/iam/ssh-key-management

## Linux File Security and Security Tools

### Storage Drive Fault Tolerance and Backup


#### RAID 1  - mirroring

2 disks at least

key points:

+ Use two drives that are mirrored
+ Data is written to both drives
+ Write speed can be slower than a single drive or RAID striping
+ read speed is the same as a single drive


#### RAID 5  - striping with parody

Key Points:

+ Uses three or more drives
+ Data is 'striped' to all drives
+ Special parity information is also striped to all drives
+ Write and read speeds can be faster than a single drive or with RAID mirroring






3 disks at least

#### RAID 6

4 disks at least


#### RAID 10 (1 and 0, not 10) and ZFS - mirroring and striping toghter


odd mirror
even mirror


zfs  - software-based rad

Key Points:

+ Combination of RAID 0 and RAID 1
+ Uses four or more drives
+ Creates two mirrors which are then stripped
+ No parity, but it is fault tolerant due to mirroring of data
+ Potentially very fast

> proxmox virtual machine

> duf


> SAS drives

> hotswappable drive








