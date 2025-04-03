# Lab: RopLab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 4/3/25


## Introduction


## ROP 100
For this challenge we were provided with a binary which I then opened in Ghidra for better analysis.
![image](https://github.com/user-attachments/assets/25784551-33a8-4c8f-a07d-75117cf21fb1)
As you can see, the main function creates a character buffer of 32 characters but reads in 56 characters. This means that there is a buffer overflow vulnerbility that we can exploit. In this program there is a win() function that executes a 'cat flag.txt' instruction.

![image](https://github.com/user-attachments/assets/328f0b2f-7830-4ed2-b842-d366506c3903)

For this challenge, we want to redirect the program to execute this separate win function. To do this, we will construct a payload to send to the program in order to manipulate the return address. First, the payload will have 40 characters (32 to fill the butter and another 8 to account for the base pointer). Then, we can manipulate the return instruction pointer (RIP) to return to the address of the win function. I got the address of the win() function by using gdb and running the command 'x win' which returned the address 0x4007bb. I then added one to this address and placed that within the payload. I then sent the complete payload to the remote service using pwntools.

Solve script:
![image](https://github.com/user-attachments/assets/c7b364c1-a3ad-470a-942c-7349fd4e715a)


Flag: --> flag{r0pp1ng_4r0und_0xc906}

## ROP 200





## ROP 300


## ROP 400



## Conclusion
