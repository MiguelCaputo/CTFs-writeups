# n33dl3

We are given a file that wen run says:

![[Pasted image 20220325130055.png]]

Strings did not show anything interesting

If we open the file on Ghidra we can see that it just calls one function in main:

![[Pasted image 20220325130207.png]]

If we see the function 

![[Pasted image 20220325130322.png]]

Nothing out of the ordinary but if we see the Function Call References we can observe that there is a lot of nested functions inside. 

![[Pasted image 20220325130407.png]]

If we start to move though each function we can see that every 34 calls there is a Mov instruction that moves one hex byte, if we collect all the hex bytes and decode them we get the flag.

Hex: 7773637b5468335f7072306d3173335f4f665f345f4b6e31736821217d 

## Flag 

> wsc{Th3_pr0m1s3_Of_4_Kn1sh!!}