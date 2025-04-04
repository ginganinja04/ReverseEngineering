# Lab: RopLab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 4/3/25


## Introduction


## ROP 100
For this challenge we were provided with a binary which I then opened in Ghidra for better analysis.
![image](https://github.com/user-attachments/assets/25784551-33a8-4c8f-a07d-75117cf21fb1)
As you can see, the main function creates a character buffer of 32 characters but reads in 56 characters. This means that there is a buffer overflow vulnerbility that we can exploit. In this program there is a win() function that executes a 'cat flag.txt' instruction, meaning that this is a ret2win challenge.

![image](https://github.com/user-attachments/assets/328f0b2f-7830-4ed2-b842-d366506c3903)

For this challenge, we want to redirect the program to execute this separate win function. To do this, we will construct a payload to send to the program in order to manipulate the return address. First, the payload will have 40 characters (32 to fill the butter and another 8 to account for the base pointer). Then, we can manipulate the return instruction pointer (RIP) to return to the address of the win function. I got the address of the win() function by using gdb and running the command 'x win' which returned the address 0x4007bb. I then added one to this address and placed that within the payload. I then sent the complete payload to the remote service using pwntools.

Solve script:
![image](https://github.com/user-attachments/assets/c7b364c1-a3ad-470a-942c-7349fd4e715a)


Flag: --> flag{r0pp1ng_4r0und_0xc906}

## ROP 200
I again used Ghidra to better analyze the binary. Again there is a buffer overflow vulnerability as the program declares a buffer of size 32 but later reads in data of size 80. In this challenge, there is a function checkDate() that contains a system() call and then returns. We want to manipulate the parameter of the system call to use it to our advantage. 


![image](https://github.com/user-attachments/assets/842e6db9-a757-4ad0-82ac-69e7f890dde7)

First, I wanted to see if there were any useful strings within the binary that I could use as a parameter for system(). I used the strings command and found that there was a string 'cat flag.txt'. I began to construct the payload, padding with 40 (32 + 8 for base pointer) characters of 'A'. Then I made sure to add the address of a pop rdi, ret instruction so that I could manipulate the parameter of the system function. I then added the address of the 'cat flag.txt' string, the address of a return instruction, and then the address of the system(). 

Solve Script:

![image](https://github.com/user-attachments/assets/0de4b722-1ea2-411f-b759-0e828873b891)


Flag --> flag{r0pp1ng_4r0und_0xc5e2c}

## ROP 300








## ROP 400



## Conclusion
