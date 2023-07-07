#tools 

= a fast online password cracking tool for [Network Services](Network%20Services.md) like telnet, rdp, ssh, ftp, http, smb ...

Example:

```
hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp

hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.38.193 ssh
```

-   -t 4 = parallel Connections to target
-   -l \[user] = useraccount name
-   -P \[ordnerpfad PWListe] = path to Directory with passwordlist
-   -vV = verbose mode (shows login+password for each try)
-   ftp = protocol