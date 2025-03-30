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

