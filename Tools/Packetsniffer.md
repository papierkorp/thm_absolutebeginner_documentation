#tools 

packetsniffer = testing if remote machine allows systemcommands / packets

# tcpdump

starting up  a tcpdump listener on the local Machine:

```
tcpdump [FLAGS] [FILTER-OPTIONEN] 
sudo tcpdump ip proto \\icmp -i eth0
```

-   -i eth0 = Networkinterface
-   proto \\icmp = Networkprotocol

Sending an ping on the Remote Machine onto my local IP:

```
.RUN ping 10.10.192.24
```

you should get an anwser on the local Machine.