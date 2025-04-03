# Lab: RopLab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 4/3/25


## Introduction
In class, we discussed how to use the tool GDB to debug and disassemble binaries.
In the lab we applied some of the uses of GDB and reversing skills in order to 
disarm a "bomb" by reversing the binary to determin the correct in order to pass the phase. 

## Rop 100
For this challenge we were provided with a binary which I then opened in Ghidra for better analysis.
![image](https://github.com/user-attachments/assets/25784551-33a8-4c8f-a07d-75117cf21fb1)
As you can see, the main function creates a character buffer of 32 characters but reads in 56 characters. This means that there is a buffer overflow vulnerbility that we can exploit. We will construct a payload to send to the program in order to access the flag. In this program there is a win() function that executes 

## Phase 2


## Phase 3


## Phase 4

## Phase 5

## Conclusion
