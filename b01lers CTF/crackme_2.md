# crackme_2

We are given a compiled program and we need to find the password

If we decompile it with Ghidra we can see the following code:

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/b01lers%20CTF/Images/crackme2.png)

We can work to solve it, we now that the first characters are:

>bctf{4lg3b

To solve the ```param_1[10] ^ param_1[9] == 16``` part we can do it backwards, we know that param_1[9] is 'b' so if we ```b ^ 16``` we get the letter "r"

>bctf{4lg3br

Now for ```param_1[11] + -1 == (int)param_1[8]``` We know that param_1[8] is the number 3, so one less will be the number 2.

Now we can finishe the flag:

> bctf{4lg3br2!} 

## Flag

> bctf{4lg3br2!}