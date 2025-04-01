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





