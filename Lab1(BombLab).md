# Lab 1: BombLab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 1/23/25


## Introduction
In class, we discussed how to use the tool GDB to debug and disassemble binaries.
In the lab we applied some of the uses of GDB and reversing skills in order to 
disarm a "bomb" by reversing the binary to determin the correct in order to pass the phase. 

## Phase 1
![image](https://github.com/user-attachments/assets/908ba45e-5931-4921-bb67-f9c5ced92969)
By disassembling the phase1 function in GDB, I was able to see the above breakdown of the function. The functions scanf, strcmp,
and complete_phase caught my attention. I used the command x/s 0x400cf9 to view the 'format' that scanf accepts, in this case it was
up to 21 characters. After, I placed a breakpoint at the strcmp and investigated the two values that were being compared, rdi and rsi.
The rdi register stored the user input, while the rsi register held the string "ghosts". If the user input matched the string "ghosts", 
then the execution would continue, jumping to offset 84 and continuing to the complete_phase function. After testing the password "ghosts", 
the password was accepted and I got the flag :triangular_flag_on_post: 	:   flag{GDB_4_th3_w1n_0xfeb49}


## Phase 2
![image](https://github.com/user-attachments/assets/7dc32fac-906d-4c00-986e-52100cc717f3)
First, I used the command x/s 0x400d19 to look at what type of value scanf was accepting, in this case it
was two integers "%d %d". After the scanf function the program had a check to ensure that more than one integers were entered. If only one was
entered, then the explode_bomb will execute. Otherwise (meaning two numbers were entered), the code continues and the values are loaded into the edx and eax registers.
At this point, edx and eax are added together, and the result is compared to 0x2ef9 (12025 in decimal). If the result is equal to 12025, execution would jump to offset 104,
and complete_phase would execute - otherwise, explode_bomb executes. Testing my theory, I entered 0 and 12025 as inputs, and got the flag :triangular_flag_on_post:flag{GDB_4_th3_w1n_0xff099}
 

## Phase 3
![image](https://github.com/user-attachments/assets/62f86218-c1b4-4e31-8a43-6996eef02418)
Again here I examing what type of values the scanf function accepted - x/s 0x400cf9, which resulted in %21s - a string with a maximum 21
characters. I stepped through the loop using the layout asm tool and found that after the loop terminated , the code compares user input with the string
"kGAItGgu" in the rsi register. The user input was accessable through the rsp register, yet each character was shifted two ascii characters up. For example, 
the string aaaa was changed to cccc after the loop terminates. I then thought to apply this shift to the string that the input is checked against, but in reverse,
shifting each character down twice. This resulted in "iE?GrEeS". Entering this string revealed the flag: :triangular_flag_on_post: flag{GDB_4_th3_w1n_0xaf0eb}

## Phase 4
![image](https://github.com/user-attachments/assets/d1a24a33-554a-4050-8f6c-b40278a631cc)
I again began by inspecting the valyue type that scanf accepted, finding that it accepted 5 integers. Like phase 2, this function had a check to see whether the user inputted
the valid amount of integers, if not, then the explode_bomb function would execute. After passing, the execution then moves into a loop, in which each decimal value is compared to the previous one
and the program checks to see if the current decical value is equal to 6 times the previous one. Once it has gone through each decimal value, comparing it with the next (until the last value), the
complete_phase function is executed and the phase is passed. For my testing, I tried the values "1 6 36 216 1296". This returned the flag: :triangular_flag_on_post:  flag{GDB_4_th3_w1n_0x77207}


## Phase 5
![image](https://github.com/user-attachments/assets/aeae5042-77ba-4dc7-b28a-06aa386c02ed)
I again began by looking at what value types scanf accepted, this was again %21s, a string of maximum length 21. The only thing that really stood out to me was the line of offset 84
with the "letters" array. In the loop the code compares each character in the user input with the corresponding character/element in the letters array. If the two match, the iteraction will continue until the counter
is greater that 17. I used the command x/55s 0x602080 to investigate what was stored in the letters array. The array stored the characters "K`tXopcieE_eqzwiIAZ". Entering this password revealed the flag :triangular_flag_on_post: flag{GDB_4_th3_w1n_0xb7110}


## Conclusion
Overall this lab did challenge my skills. Often, I found that I was on the right track but lacked knowledge of small details that the challenges required. 
After guidance from Marcus and John, it was clear that I was correct in my overall process but just lacked some of the essential background knowledge. Their walkthroughs helped a lot.

