# Redo1

We are given some C code and we are supposed to guess the flag but we can modify the code like:

```
for(int i = 0; i < STR_LEN; i++)
{
	int idx = i;
	if(i >= 4 && i <= 15){ idx += 4; }
	if(i >= 16 && i <= 23){ idx += 8; }
	if(i > 23){ idx += 12; }
	printf("%d\n", flag[idx]);
	//if(argv[1][i] != flag[idx]){ EXIT }
}
```

And compile it like ```gcc redo1.c```

So it we run the code with any 34 digit parameter it will print the digits of the flag:

```
103
105
103
101
109
123
66
52
83
49
67
95
67
95
108
97
110
71
117
65
103
69
95
82
69
95
48
120
71
76
65
83
83
125
```

We can convert them to characters and get the flag.

## Flag

> gigem{B4S1C_C_lanGuAgE_RE_0xGLASS}