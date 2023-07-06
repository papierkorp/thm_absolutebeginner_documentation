=> Hash Cracker, has a lot of different distribution. Most popular distribution is "Jumbo John".

# Install

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

[Windows Authentication](Enumeration/Windows.md#authentication)

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

## Single Crack Mode

John uses only the Information provided in the Username to try and work out possible passwords heuristically by word mangling.

for Example the Username: `Markus`

could be:

-   Markus1, Markus2, Markus3 (etc.)
-   MArkus, MARkus, MARKus (etc.)
-   Markus!, Markus$, Markus* (etc.)

John can also use Geco Fields of UNIX OS. (extra Information in `/etc/shadow` / `/etc/passwd`).

Use the Single Crack Mode:

```bash
john --single --format=[format] [path to file]
john --single --format=raw-sha256 hashes.txt
```

while changing the hash from `1efee03cdcb96d90ad48ccc7b8666033` to `mike:1efee03cdcb96d90ad48ccc7b8666033`


## Custom Rules

Define your own password patterns in `/etc/john/john.conf or /opt/john/john.conf`

[John Wiki](https://www.openwall.com/john/doc/RULES.shtml)

The `john.conf` should look like this:

```bash
[List.Rules:PoloPassword] #first line is the name of the rule, used as a argument
cAz"[0-9] [!£$%@]" #next following is a regex pattern to define which word will be modified
```

Explanation of the Regex:
- `Az` Append to the end of the word
- `A0` Takes the word and prepends it with the characters you define
- `c` Capitalise the first  letter
- `[0-9]` - A number in the range 0-9
- `[!£$%@]` - Followed by a symbol that is one of

For example, you have following rules: 

- contain at least one of the following:
	- Capital letter
	- Number
	- Symbol

Many users will use Polopassword1! => Capital letter first, followed by a number and a symbol at the end.

To use our defined CustomRule: `john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]`


## Zip2John (crack zip files)

We have to convert the zip File to a hah format that John is able to understand.

`zip2john [options] [zip file] > [output file]`

- `[options]` - Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary  
- `[zip file]` - The path to the zip file you wish to get the hash of
- `>` - This is the output director, we're using this to send the output from this file to the...  
- `[output file]` - This is the file that will store the output from

`zip2john zipfile.zip > zip_hash.txt`

and then `john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt`

## Rar2John (crack rar files)

The same as [zip2john](#zip2john_(crack_zip_files))

`/opt/john/.rar2john [rar file] > [output file]`

`/opt/john/.rar2john rarfile.rar > rar_hash.txt`

`john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt`


## ssh2john (crack ssh key passwords)

The same as [zip2john](#zip2john%20(crack%20zip%20files))

`ssh2john [id_rsa private key file] > [output file]`
`ssh2john id_rsa > id_rsa_hash.txt`

if ssh2john isnt installed use:

`python3 /opt/ssh2john.py id_rsa > id_rsa_hash.txt` / `python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt` instead.

Finish with: `john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt`

# Examples

Exploit MySQL with [MetaSploit](metasploit)

```bash
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