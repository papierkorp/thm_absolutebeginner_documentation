# GoBuster

Directory Brute Forcer.

```
sudo apt install gobuster
```


```
root@ip-10-10-3-138:~# gobuster dir -u http://shell.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://shell.uploadvulns.thm
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2022/10/05 08:38:12 Starting gobuster
===============================================================
/resources (Status: 301)
/assets (Status: 301)
/server-status (Status: 403)
===============================================================
2022/10/05 08:53:22 Finished
===============================================================
```

Parameter
- `-x php,txt,html` append each word to the selected wordlist one at a time (if the name of the uploaded file is changed) 

# Wappalyzer

Wappalyzer works with the tools you use every day.

Install the free browser extension to see the technologies used on websites you visit or install Wappalyzer in your CRM to see the technologies used by your leads.

https://www.wappalyzer.com/apps