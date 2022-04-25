# gambler_baby

We are given a program that asks for a lowercase 4 letter input and if we match it to a "random" 4 letter input we are given some coins, we need to get enough points to get to 1000, the thing is that the program is not really random so it inputs the same strings everytime, we can create a python program to produce all the words

```
from pwn import *

while True:
    p = process('./gambler-baby1', level='error')
	
	for word in list:
		p.recvuntil(b'letters: ')
		p.sendline(word)
		
	for i in range(10):
		p.recvuntil(b'letters: ')
		p.sendline(b'aaaa')
		p.recvuntil(b'Correct word: ')
		newletter = p.recvline()[:-1]
		list.append(newletter)
		print(list)
		
	p.close()
```

Now that we have the list of words we can get the flag with the following python program:

```
from pwn import *

# List of words

list = [b'nwlr', b'bbmq', b'bhcd', b'arzo', b'wkky', b'hidd', b'qscd', b'xrjm', b'owfr', b'xsjy', b'bldb', b'efsa', b'rcby', b'necd', b'yggx', b'xpkl', b'orel', b'lnmp', b'apqf', b'wkho', b'pkmc', b'oqhn', b'wnku', b'ewhs', b'qmgb', b'buqc', b'ljji', b'vswm', b'dkqt', b'bxix', b'mvtr', b'rblj', b'ptns', b'nfwz', b'qfjm', b'afad', b'rrws', b'ofsb', b'cnuv', b'qhff', b'bsaq', b'xwpq', b'cace', b'hchz', b'vfrk', b'mlno', b'zjkp', b'qpxr', b'jxki', b'tzyx', b'acbh', b'hkic', b'qcoe', b'ndto', b'mfgd', b'wdwf', b'cgpx', b'iqvk', b'uytd', b'lcgd', b'ewht', b'acio', b'hord', b'tqkv', b'wcsg', b'spqo', b'qmsb', b'oagu', b'wnny', b'qxnz', b'lgdg', b'wpbt', b'rwbl', b'nsad', b'eugu', b'umoq', b'cdru', b'beto', b'kyxh', b'oach', b'wdvm', b'xxrd', b'ryxl', b'mndq', b'tukw', b'agml', b'ejuu', b'kwci', b'bxub', b'umen']


## Code To Win

p = remote('ctf.b01lers.com','9202', level='error')

for word in list:
    p.recvuntil(b'letters: ')
    p.sendline(word)

print(p.recvall())

p.close()
```

# Flag: 

> bctf{n0_pr4153_f0r_RNGesus}