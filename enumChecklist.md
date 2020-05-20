# Hitchs Enumeration Checklist

## Initial Recon:

Network Scan - nmap -sV -sC -oN $IP


## External Facing Services:

### HTTP/HTTPS:

Subdirectory fuzzing - gobuster dir -u http://$IP -w /usr/share/wordlists/WORDLIST  
Check robots.txt  
Check if running common web application software (wordpress, CMS, etc.)  
If HTTPS, check certificate for details  
Look for login pages  
Look for hostname  
Look for upload form  
Look for SQLI / XSS / LFI / RFI  

### SSH:

Connect to SSH with private key - ssh -i privKey user@$IP  
Setting correct private key permissions - chmod 600 privKey  
  
If < OpenSSH 7.7 use auxiliary/scanner/ssh/ssh_enumusers to get SSH users  
Check for other version exploits  
  
Brute force password (slow) - hydra -l user -P /usr/share/wordlists/rockyou.txt $IP ssh  

### FTP:

Connect as anonymous user - ftp anonymous@$IP  
Check for version exploits  

### SMB:

Connect as anonymous user - smbclient -L $IP  
Enumerate shares - smbmap -H $IP  
Check for version exploits  

### DNS:

Perform DNS zone transfer - dig axfr domain-name @nameserver  
Brute force DNS subdomains - dnsrecon -d hostname.url -D /usr/share/wordlists/WORDLIST -t std  

### MySQL:

Connect to MySQL server - mysql --user=username --password=password --host=$IP  


## Internal User access (Linux):

Upgrade shell - python -c "import pty;pty.spawn('/bin/bash')"  
What user you are - whoami  
User path - echo $PATH  
System version - uname -ar  
Users home directory - ls -la ~  
Sudo permissions for user - sudo -l  
Enemurate other users - cat /etc/passwd  
Password files permissions - ls -la /etc/passwd /etc/shadow  
SSH key files / config files (/home/user/.ssh /etc/ssh)  
SUID binaries - find / -perm -u=s -type f 2>/dev/null  
Search for GTFOBins Binaries - ls -la (/bin, /usr/bin, /sbin, /usr/sbin)  
Configuration files - find / -name "*conf*" -type f 2>/dev/null  
Backup files - find / -name "*.bak" -type f 2>/dev/null  
World writeable folders - find / -perm -o w -type d 2>/dev/null  
Writeable files that user doesnt own - find / -writable ! -user user -type f 2>/dev/null  
Currently running services - ps aux  
Environment variables - cat (/etc/profile, /etc/bashrc, ~/.bash_profile, ~/.bashrc, ~/.bash_logout)  
Cron jobs - crontab -u user -l  
Root home directory permissions - ls -la /root  


# Current Versions of Common Services / Programs:

## Perimeter Facing Services / Programs:
OpenSSH 8.2  
Apache httpd 2.4  
NGINX 1.17.0  
MySQL 8.0.20  
VSFTPD 3.0.3  
  

## Internal Services / Programs
Linux Kernel 5.6.13  
Sudo 1.9.0  
