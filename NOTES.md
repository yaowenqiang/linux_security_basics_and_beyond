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



