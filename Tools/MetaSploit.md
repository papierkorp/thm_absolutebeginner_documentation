#tools

# General

Tool used for everything in Penetration Testing

Exploit = Code to take advantage of Vulnerabilities on a Target System
Vulnerability = Security Weakness on a Target System
Payload = Code which will be run on the Target System

Usage + Examples:

```
msfconsole

msf5> help set
msf5> history
msf5> use exploit/windows/smb/ms17_010_eternalblue
msf5 exploit(windows/smb/ms17_010_eternalblue)> show options
msf5 exploit(windows/smb/ms17_010_eternalblue)> show payloads
msf5 exploit(windows/smb/ms17_010_eternalblue)> info
msf5 exploit(windows/smb/ms17_010_eternalblue)> back
msf5> info exploit/windows/smb/ms17_010_eternalblue
msf5> search ms16-010
msf5> search type:auxiliary telnet
msf5> search apache
msf5> set PARAMETER_NAME VALUE
msf5> show options
msf5> unset all
```


**Ranking**

-   Excellent = exploit will never crash a server, no Memory corruption
-   Great = exploit has a defaul target and recognizes it automatically or uses a app specific return adresse after the version check
-   Good = exploit has a default Target (is the default)
-   Normal = exploit should be reliable, but is dependet on a specfic version and has no autodetect
-   Average = exploit is rather unreliable or hard to exploit
-   Low = under 50% success rate
-   Manual = exploit is unsecure or hard to exploit

**Common Parameters**

-   RHOSTS = Remote host, IP address of Targetsystem
-   RPORT = Remote port, Port of  Targetystem
-   PALOAD = payload der benutzt werden soll
-   LHOST = Localhost, IP Adresse vom Angreifer
-   LPORT = Localport, Port vom Angreifer
-   SESSION = session ID

```
//set parameter only for the specific module you currently are in
set RHOSTS 10.10.10.10
unset RHOSTS
//set global parameter for all modules
setg LHOST 10.10.10.1
unsetg all
```



# Session

As soon as a exploit was successfull a Session will be created.
Serves as a Kommunicationchannel between the Targetsystem and Metasploit

```
background
sessions
sessions -i 1
meterpreter>
```

- background = go back to msf5>  (alternative: ctrl+z)
- sessions = display all active sessions
- sessions -i 1 = join a specific session

# Modules

Using Modules:

```
msf5 exploit(windows/smb/ms17_010_eternalblue)> exploit -z
```

-   exploit = you can also use the alias run
-   -z = background
-   check = not availabel everywhere, checkt if the system is vulnerable

## Support

Scanner/Crawler/Fuzzers

```
/opt/metasploit-framework-5101/modules# tree -L 1 auxiliary/
```
-   admin
-   analyze
-   bnat
-   client
-   cloud
-   crawler
-   docx
-   dos
-   example.rb
-   fileformat
-   fuzzers
-   gather
-   parser
-   pdf
-   scanner
-   server
-   sniffer
-   spoof
-   sqli
-   voip
-   vsploit

### smtp_version

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

### mysql

**mysql_sql**

```
search mysql_sql
use auxiliary/admin/mysql/mysql_sql
options
set RHOSTS 10.10.60.123
set USERNAME root
set PASSWORD password
exploit
set SQL show databases
exploit
```

Output:

```
mysql version + tables
```


**mysql_schemadump**
```
search mysql_schemadump
use auxiliary/scanner/mysql/mysql_schemadump
set RHOSTS 10.10.60.123
set USERNAME root
set PASSWORD password
exploit
```

Output:

```
...
  - TableName: x$waits_by_user_by_latency
    Columns:
    - ColumnName: user
      ColumnType: varchar(32)
    - ColumnName: event
      ColumnType: varchar(128)
    - ColumnName: total
      ColumnType: bigint(20) unsigned
    - ColumnName: total_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: avg_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: max_latency
      ColumnType: bigint(20) unsigned
  - TableName: x$waits_global_by_latency
    Columns:
    - ColumnName: events
      ColumnType: varchar(128)
    - ColumnName: total
      ColumnType: bigint(20) unsigned
    - ColumnName: total_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: avg_latency
      ColumnType: bigint(20) unsigned
    - ColumnName: max_latency
      ColumnType: bigint(20) unsigned
```


## Encoder

encode Expolit Code / Payload Code to evade Firewalls

```
/opt/metasploit-framework-5101/modules# tree -L 1 encoders/
```

-   cmd
-   generic
-   mipsbe
-   mipsle
-   php
-   ppc
-   ruby
-   sparc
-   x64
-   x86

## Evasion

Trying to directly avoid Firewalls/Antivirus

```
/opt/metasploit-framework-5101/modules# tree -L 2 evasion/
```

-   applocker_evasion_install_util.rb
-   applocker_evasion_msbuild.rb
-   applocker_evasion_presentationhost.rb
-   applocker_evasion_regasm_regsvcs.rb
-   applocker_evasion_workflow_compiler.rb
-   windows_defender_exe.rb
-   windows_defender_js_hta.rb

## Exploit

```
/opt/metasploit-framework-5101/modules# tree -L 1 exploits/
```

-   aix
-   android
-   apple_ios
-   bsd
-   bsdi
-   dialup
-   example_linux_priv_esc.rb
-   example.rb
-   example_webapp.rb
-   firefox
-   freebsd
-   hpux
-   irix
-   linux
-   mainframe
-   multi
-   netware
-   openbsd
-   osx
-   qnx
-   solaris
-   unix
-   windows

## NOPs (No Operation)

Dont do anything. Are used as Buffer to get consistent Payload Sizes.

```
/opt/metasploit-framework-5101/modules# tree -L 1 nops/
```

-   aarch64
-   armle
-   mipsbe
-   php
-   ppc
-   sparc
-   tty
-   x64
-   x86

## Payloads

Code designed to run on the Targetsystem.
for example to get a shell, to load Malware/a Backdoor, to run a command or to open a software

```
/opt/metasploit-framework-5101/modules# tree -L 1 payloads/
```

-   singles
    -   self-contained (doesnt need any additional component)
    -   add user, launch notepad.exe ..
    -   generic/shell_reverse_tcp
-   stagers
    - build up a connection between Metasploit and the Targetsystem  
    -  used with staged payloads (first upload stager and then the  download rest of the payload = stage)
-   stages
    -   downloaded by stager
    -   allows bigger payloads
    -   windows/x64/shell/reverse_tcp

## Post

Used at the end of a Penetration Testing Process (post-explotation)

```
/opt/metasploit-framework-5101/modules# tree -L 1 post/
```

-   aix
-   android
-   apple_ios
-   bsd
-   firefox
-   hardware
-   linux
-   multi
-   networking
-   osx
-   solaris
-   windows