# Francisco Torres

We need to pwn an app that returns QR codes and we need to input the QR code value in a timeframe, tried to automatize it but I couldnt properly read the QR codes so I did it by hand with pwntools

Python Code

```
from pwn import *

def send(payload, r):
 r.recvuntil(b">")
 r.sendline(payload.encode("utf-8"))

curr = "flag{75C04"
fl = ""
r = remote('3.145.216.156', '41879', level='error')

for c in curr:
 fl += c
 send(fl, r)

  

send(fl, r)

send('flag{7', r)

send('flag{75C0447B', r)

send('flag{75C0447B6', r)

send('flag{75C0447B6@', r)

send('flag{75C0447B', r)

send('flag{75C0', r)

send('flag{75C0447B6@B01', r)

send('flag{75C0447B6@B013', r)

send('flag{75', r)

send('flag{75', r)

send('flag{75C0447B6@B01355A', r)

send('flag{75C0447B6@B01355AE', r)

send('flag{75C0447B6@B01355AEd', r)

send('f', r)

send('flag{75C0', r)

send('flag{75C', r)

send('flag{75C0447B6@B01355AE', r)

send('flag{75C0447B6', r)

send('flag{75C0447B6@B01355AEdf0CE6F', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75', r)

send('flag{75C0447B6@', r)

send('flag{75C0', r)

send('flag{75C0447B6', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b', r)

send('flag{7', r)

send('flag{75C0', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b', r)

send('flag{75C0', r)

send('flag{75C0447B6@B01355AEd', r)

send('flag{7', r)

send('flag{75C0447B6@B01355AEdf0CE6F', r)

send('flag{7', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78', r)

send('flag{75C04', r)

send('flag{75C0447B6@B013', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e', r)

send('flag{75C0447B6@B013', r)

send('f', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78', r)

send('flag{75C04', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c', r)

send('flag{75', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D', r)

send('flag{75C0447B6@B013', r)

send('flag{75C04', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b', r)

send('flag{75C0447B6@', r)

send('flag{75C0447B', r)

send('flag{75C0447B6@B013', r)

send('flag{75C0', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C04', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75C04', r)

send('flag{75', r)

send('flag{75C0447B6', r)

send('flag{75', r)

send('flag{75', r)

send('flag{75', r)

send('flag{75', r)

send('flag{75C0447B6@B01', r)

send('flag{75C0447B6@B013', r)

send('flag{75C04', r)

send('flag{75C04', r)

send('flag{75C0447B6@', r)

send('flag{75C0447B6', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78', r)

send('flag{75C0447B6', r)

send('flag{75C04', r)

send('f', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F7889', r)

send('flag{75', r)

send('flag{75', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c', r)

send('flag{75C0447B6@B01', r)

send('flag{7', r)

send('flag{75C0447B6@B01355AEdf0CE6F2', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_H', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$t', r)

send('fla', r)

send('flag', r)

send('flag{75C0447B6@B013', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$tag3_o', r)

send('f', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$t', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_H', r)

send('flag{75C0447B6@B013', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$tag3_of_tH3_Y', r)

send('flag{75C0447B6@B01355AE', r)

send('flag{75C0447B6@B01355A', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$tag3_of_tH3_YEAr', r)

send('flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$tag3_of_tH3_YEAr}', r)

print(r.recvall())

r.close()
```

## Flag

> flag{75C0447B6@B01355AEdf0CE6F225@06b70b0d7F78899843e3f84c592D34b@B30D94245655551344@692c9864f955c172_HO$tag3_of_tH3_YEAr}