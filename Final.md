# Final
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 05/02/2025


## Final 1 - Ret2Win
For this challenge, we were given a binary that contained a vuln function. This vuln function contained a loop that presented options to the user. The binary also contained a display_flag and a feed_greg function. The feed_greg function would call the display_function if the two parameters matched some comparison values. I knew then I needed to manipulate rdi and rsi values and manipulate the rip to match the address of feed_greg. I used a rop chain to achieve this, padding with 40 to cover the buffer and the base pointer. I ensured that the rsi and rdi registers matched the values that feed_greg compared to. 

![image](https://github.com/user-attachments/assets/7447a363-0c46-4d98-b249-8df03d8cbe4d)


Running the above script returned the flag --> fitsec{feline_final_0xba94c}


## Final 2 - Ret2LibC
For this challenge, we were provided with a similar binary in which users were presented with options in a loop. We were also presented with a pointer to the printf function. Similarly to before, we need to construct a rope chain that allows us to manipulate the flow of execution of the program and manipulate function parameters. The provided printf function allows us to calculate the base libc address, which gives us access to some functions and gadgets we can use to our advantage. After calculating the base address, we can grab some useful strings such as '/bin/sh' and the gadget 'pop rdi, ret'. We can grab the appropriate offset addresses in libc we need using gdb and add that to the base address to get the correct address of the gadgets and strings we need. After this, we just need to put everything together. First we pad the payload with 40 again to account for the buffer and rbp, then add our rdi gadget address, then the /bin/sh address, then a ret gadget, and then our system function. After, we can send the input and payload using pwn tools, giving us our shell. Once inside the shell, we can run 'cat flag.txt' to see the flag.

![image](https://github.com/user-attachments/assets/7e95e738-2cd2-42b1-810b-c169d631bfb4)

![image](https://github.com/user-attachments/assets/39e90763-7501-405f-a260-84b093bb808d)

flag --> fitsec{feline_final_0x275c}


I originally had some issues with this challenge. I was very close to getting the solution, but found that changing to using the address offsets and base addresses instead of using the ROP tool in Python helped me get to the flag. Also, I believe the pwn tools sendline and recvuntil tools were not working how I anticipated, so that may have caused some issues as well. 
