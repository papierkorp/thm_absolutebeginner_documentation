# NMap 

Is controlled with Console Commands:

`nmap [Scan Type] [Options] {target}</code>`

Scan Types
* Most used
  * TCP Scan
    * uses a Three-Way Handshake (SYN, SYNACK, ACK)
    * if Port is closed we get a Reset (RST) Flag
    * if Port is behind a Firewall/filtered: no Anwser (could possibly configured with a RST Flag)
    * Usage
        * `nmap -sT`
  * Syn Scan
    * Half Open / Stealth Scan
    * Three Way Handshake, but we send a RST instead of a ACK
    * is mostly not logged because there is no complete Handshake
    * faster than TCP Scans, needs sudo (for the RST Packet), could possibly kill the Service
    * Usage
      * `nmap -sS `
  * UDP Scan
    * stateless (wont build up a Connection)
    * sends packets to Targetport and hopes for an anwser
    * if no anwser = open/filtered
    * if UDP anwser = open
    * if ICMP ping = closed
    * slower, because we dont get an anwser and send more packets
    * will be used with `--top-ports 20` most of the time
    * Usage
      * `nmap -sU`
* Firewall evasion
  * NULL Scan
    * sends TCP Request without Flags
    * `-sN`
  * FIN Scan
    * sends with FIN flag
    * `-sF`
  * Xmas Scan
    * sends malformed TCP packet with PSH, URG and FIN Flag
    * `-sX `
* Others
  * Ping Sweep
    * doenst Scan Ports
    * uses ICMP echo packets / ARP requests
    * to get an Overview about all IP Addresses in a Segment (not 100% accurate)
    * Usage
      * `nmap -sn 192.168.0.1-254`
      * `nmap -sn 192.168.0.0/24`
  * Service Scan
    * `nmap -sV`

Options
* find Operating System
  * `nmap -O`
* save Output  (oA = 3 major Formate, oN = normal, oG = grepable ..)
  * `nmap -oA output.txt`
* Aggressive (Service, OS, Tracerout + Script Scanning)
  * `nmap -A`
* Timing - makes the Scan faster, but with possible Errors (0-5, 5 highest)
  * `nmap -T5`
* Scan for a specific Port / all Ports
  * `nmap -p 80`
  * `nmap -p-`
* add scripts
  * `nmap --script`
* verbosity / Extended
  * `nmap -v / nmap -vv`

How to Evade the Firewall
* No Ping before the scan (so the Port isnt counted as dead)
  * workaround an ICMP block, scan needs more time
  * Usage
    * `nmap -Pn`
* split packets into fragments
  * more unlikey for the packets to be recognized, since size, quantity and timing will be reduced
  * evade Firewall/IDS Trigger which are time-based
  * badsum creates invalid checksum
  * Usage
    * `nmap -f`
    * `nmap -mtu <number multiple of 8>`
    * `nmap --scan-delay <time>ms`
    * `nmap --badsum`

**Examples**

```
nmap 10.10.82.210
nmap -sp 10.10.82.0/24
nmap --top-ports 20 10.10.82.210
nmap -p 1-65535 10.10.82.210
nmap -sX -p 0-999 -vv 10.10.167.84
nmap -sV -A -oN nmapscanoutput.out 10.10.82.210
```



## NSE (Nmap Scripting Engine)

Part of [NMap](#NMap) - Scripts are written in LUA

Besides own written Scripts, there is a exisiting [Library](https://nmap.org/nsedoc/) with the following categories: 

* safe = Target wont be influenced
* intrusive = not safe, can influence Target
* vuln = Scan for vulnerabilities
* exploit = exploit vulnerabilities
* auth = evade authentication (Login) Systems
* brute = Bruteforce Data for running services
* discovery = search for other services

Script Folder in THM VM:

```
cd usr/share/nmap/scripts/
grep "safe" /usr/share/nmap/scripts/scripts.db
ls -l /usr/shar/nmap/scripts/*ftp*
sudo wget -O /usr/share/nmap/scripts/\<script-name\>.nse [NSE Skripte](https://svn.nmap.org/nmap/scripts/)
nmap --script-updatedb
```

Usage:

```
nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'
```

* script and script-args are sperated with commas
