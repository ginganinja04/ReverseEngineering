# Lab 2: Math Lab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 02/06/25


## Introduction
In class, we discussed how to use Ghidra to disassemble and decompile binaries statically. In this lab, you will be applying your knowledge to successfully solve each of the math-themed challenge binaries.


## Math-100
For this challenge we could see using Ghidra that the binary was taking two "random" numbers and 
computing a rolling sum of the two numbers. ![image](https://github.com/user-attachments/assets/c92375b7-e1c0-4742-b61d-04a74984a308)

Using a Python script and pwn-tools, we could recreate this process to ensure that the user input
matched the value that the binary was checking against to get the flag.

```
from pwn import *
p=remote("172.233.177.217", 10520)

p.recvuntil(">>>")
num1 = p.recvline().decode("ascii").strip()
num1 = int(num1)

p.recvuntil(">>>")
num2 = p.recvline().decode("ascii").strip()
num2 = int(num2)

for i in range(100):
        num1 += num2

p.recvuntil(">>>")
p.sendline(b"%d"%num1)

p.interactive()
```
Executing this code revealed the flag:  flag{Gh1dr4_4dd1ct_0x7dd92}

## Math-200

![image](https://github.com/user-attachments/assets/33b9b89a-088b-42c1-aa24-750b40568f57)

This challenge was very similar to challenge one in that it took two "random" numbers and 
checked the result of a computation with those numbers against user input. For this challenge, 
the check was passed if the input = (num2 + num1) * (num1 - num2). Again implementing this with a 
Python script :

```
from pwn import *
p=remote("172.233.177.217", 10521)

p.recvuntil(">>>")
num1 = p.recvline().decode("ascii").strip()
num1 = int(num1)

p.recvuntil(">>>")
num2 = p.recvline().decode("ascii").strip()
num2 = int(num2)

num3 = (num2 + num1) * (num1 - num2)

p.recvuntil(">>>")
p.sendline(b"%d"%num3)

p.interactive()
```
 This revealed the flag:  flag{Gh1dr4_4dd1ct_0x9cded}

## Math-300

![image](https://github.com/user-attachments/assets/90312989-cc18-49d9-bdda-d59c9e6f4ebc)

![image](https://github.com/user-attachments/assets/e131651c-a85c-444d-99b9-68f6803e0146)

![image](https://github.com/user-attachments/assets/a95fafe0-7a88-4e49-941c-51ea87276afa)


This challenge was a bit more complex from the previous ones as it required some algebra but followed the same structure of taking two "random" integers and performing computations with those numbers. This binary performed a loop, and within the loop it called a function that would check a calculated number against the value 1337 (local_10). Within the checker function, the function would perform different math equations using num1, num2, current i value from the loop, and user input based on the result of a previous mod operation. If the result of those calculations resulted in 1337, the check would pass. However, we want to find out what the correct input would be to send. Since we know all of the other values, we can solve for the correct input by simply rearranging the algebra and performing some quick calculations.

```
from pwn import *
p=remote("172.233.177.217", 10522)


for i in range (1,100):

  p.recvuntil(">>>")
  num1 = p.recvline().decode("ascii").strip()
  num1 = int(num1)

  p.recvuntil(">>>")
  num2 = p.recvline().decode("ascii").strip()
  num2 = int(num2)

  target = 1337
  input_value = 0

  if i % 10 < 9:
    if i % 10 == 0:
      input_value = target + num2 - i - num1
    elif i % 10 == 1:
      input_value = num2 + i + num1 - target
    elif i % 10 == 2:
      input_value = target - num1 + num2 + i
    elif i % 10 == 3:
      input_value = -1*(target - num1 - i + num2)
    elif i % 10 == 4:
      input_value = num1 + num2 - i - target
    elif i % 10 == 5:
      input_value = target - i - num1 + num2
    elif i % 10 == 6:
      input_value = target - num1 - num2 + i
    elif i % 10 == 7:
      input_value = num2 + i + num1 - target
    elif i % 10 == 8:
      input_value = target - num1 + num2 + i
  else:
      input_value = target - num1 - num2 - i

  p.recvuntil(">>>")
  p.sendline(b"%d"%input_value)

p.interactive()

```
This revealed the flag: flag{Gh1dr4_4dd1ct_0xa0936}

## Conclusion 
I felt that the first two challenges were pretty straightforward. However, the third challenge was a bit more challenging. I knew what needed to happen to calculate the input, but struggled with the 
implementation a bit in Python, and at first, I forgot to consider the loop in my script. Once I 
realized that, I fixed it and got the flag. 
