#mails #enumeration 

# Metasploit

Example:

```
msfconsole
search smtp_version
use auxiliary/scanner/smtp/smtp_version
options
set RHOSTS 10.10.38.193
exploit
back
use auxiliary/scanner/smtp/smtp_enum 
set USER_FILE /usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txt
set RHOSTS 10.10.38.193
exploit
```

# smtp-user-enum