# babyre1

We are given a binary, if we analyze it on Ghidra we can see

```
int main(void) {
  puts("Wait did I encode the flag already?");
  encode("Wrong Input");
  return 0;
}
```

If we follow the encode function we can see:

```
void encode(char *input) {
  size_t sVar1;
  char result;
  int i;
  
  i = 0;
  while( true ) {
    sVar1 = strlen(input);
    if (sVar1 <= (ulong)(long)i) break;
    putchar((int)(char)(input[i] ^ 0x3b));
    i = i + 1;
  }
  return;
}
```

If we search strings we can see the encoded flag:

![image](https://github.com/MiguelCaputo/CTFs-writeups/blob/main/WolvSec%202022/Images/encoded.png)
 
 We can create a python script to decode the flag:
 
 ```
 decode = ['L', 'H', 'X', '@', 'b','\v', 'N', 'd', '\x0F', 'I','\b', 'd', '\\','\b', 'O', 'O', '\n', 'U','\\', 'd', 'O', 'S','\b', 'd', 'S', '\x0F', 'U','\\', 'd','\v', ']', 'd', 'o', 's', 'r', 'h', '\x1a', 'F']


for char in decode:
 	cVar1 = ord(char)
 	x = chr(cVar1 ^ 0x3b)
 	print(x, end="")
```

## Flag

> wsc{Y0u_4r3_g3tt1ng_th3_h4ng_0f_THIS!}