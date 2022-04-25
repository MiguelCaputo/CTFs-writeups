# extreme_64

We are given a netcat connection where we need to solve some x86_64 assembly problems:

## Problem 1
Set rdi to 0x1337 using only one instruction.

#### Solution 

```
mov rdi, 0x1337;
```

#### Level Password
> code{very_1337}

## Problem 2
Add rdi to rsi and store the result in rax using two or less instructions.

#### Solution 

```
add rdi, rsi;
mov rax, rdi
```

#### Level Password

> code{math_time}

## Problem 3
Transform the following C code to assembly:
```
if (rax == 0x1000) {
	rsi = 0x10;
}
```

#### Solution 

```
cmp rax, 0x1000;
jne L2;
L1:
mov rsi, 0x10;
L2:
;
```

## Problem 4
Transform the following C code to assembly:
```
if (rax == 0x1000) {
	rsi = 0x10;
} else if (rax == 0x3000) {
	rsi = 0x20;
}
```

#### Solution 

```
cmp rax, 0x1000;
je L1;
cmp rax, 0x3000;
jne L3;
L2:
mov rsi, 0x20;
jmp L3;
;
L1:
mov rsi, 0x10;
;
L3:
;
```

#### Level Password

> code{we_c4n_d0_th1s_all_d4y}

## Problem 5
Transform the following C code to assembly:
```
while (rax > 0x0) {
	rsi += rax;
	rax--;
}
```

#### Solution

```
jmp L2;
L3:
add rsi, rax;
sub rax, 1;
L2:
cmp rax, 0;
jg L3;
```

#### Level Password
> code{l00p_the_l00p}

## Flag

> bctf{c3rt1f13d_asm_pr0gr4mmer!!}
