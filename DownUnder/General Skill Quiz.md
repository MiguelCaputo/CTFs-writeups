# General Skill Quiz

We are given a netcat connection "nc pwn-2021.duc.tf 31905" and we are asked basic questions that we have 30 seconds to answer.

# Automate the answers

We can automate the answers to the host using python and the pwn library

I wrote the following script for that:

```
from pwn import *
from urllib.parse import unquote
import base64
import codecs

rot13 = lambda s : codecs.getencoder("rot-13")(s)[0]

r = remote('pwn-2021.duc.tf', 31905)

print (r.recvline().decode("utf-8"))
r.send(b"Hello world!\n")
r.send(b"2\n")

print (r.recvuntil(b"0x"))
hex = int (r.recvline().decode("utf-8"), 16) 
r.send(("{}\n".format(hex)).encode("utf-8"))

print (r.recvuntil(b"ASCII letter: "))
hex = r.recvline().decode("utf-8")
hex = bytes.fromhex(hex).decode("utf-8")
r.send(("{}\n".format(hex)).encode("utf-8"))

print (r.recvuntil(b"ASCII symbols: "))
url = r.recvline().decode("utf-8")
url = unquote(url)
r.send(("{}".format(url)).encode("utf-8"))

print (r.recvuntil(b"plaintext: "))
base = r.recvline()
base = base64.b64decode(base).decode("ascii")
r.send(("{}\n".format(base)).encode("utf-8"))

print (r.recvuntil(b"Base64: "))
base = r.recvline()
base = base64.b64encode(base[:-1]).decode("ascii")
r.send(("{}\n".format(base).encode("utf-8")))

print (r.recvuntil(b"plaintext: "))
rot = r.recvline().decode("utf-8")[:-1]
r.send(("{}\n".format(rot13(rot)).encode("utf-8")))

print (r.recvuntil(b"equilavent:"))
rot = r.recvline().decode("utf-8")[:-1][1:]
r.send(("{}\n".format(rot13(rot)).encode("utf-8")))

print (r.recvuntil(b"(base 10): "))
base_ten = int(r.recvline().decode("utf-8")[:-1],2)
r.send(("{}\n".format(base_ten).encode("utf-8")))

print (r.recvuntil(b"equivalent: "))
base_ten = bin((int) (r.recvline().decode("utf-8")[:-1]))
r.send(("{}\n".format(base_ten).encode("utf-8")))

print (r.recvuntil(b"universe?\n"))
r.send(("DUCTF\n".encode("utf-8")))
print(r.recvall())

r.close()
```

# Flag

> DUCTF{you_aced_the_quiz!_have_a_gold_star_champion}
