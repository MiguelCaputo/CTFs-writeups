# An Unknown Disassembly

We are given some python dissasembly code and we need to get the flag, we can use the ```dis``` python library to create code and dissasembly it so we can create a program similar to the given one, the program that I created was:

```
import dis

def test():
 a = input('Enter the super secret password:')
 b = ''
 c = 0
 for x in a:
 	if x == 'a':
 		b += '@'
 	if x == '@':
 		b += 'a'
 	elif x == 'o': 
 		b += '0'
 	elif x == '0':
 		b += 'o'
 	elif x == 'e':
 		b += '3'
 	elif x == '3':
 		b += 'e'
	elif x == 'l':
 		pass
 	else:
		b += x
 	c += 1
 d = 'S0th3combination1sonetw0thr3efourf1ve'
 if c % 4 == 0:
 	if b == d:
 		print('You got the flag!')
 		exit()
 print('Oops, try again.')

dis.dis(test)
```

This code produces an assembly identical to the one given, we can see that some letter are changed and if the letter is "l" it is ignored, so we can modify the string 'S0th3combination1sonetw0thr3efourf1ve' with the changes and add some "l" at the end to make the count of caracters % 4 == 0

Final string:

> Sothec0mbin@ti0n1s0n3twothre3f0urf1v3lll

## Flag

> shctf{1m_jUst_a_p14in_y0gurt_ch4l1eng3}