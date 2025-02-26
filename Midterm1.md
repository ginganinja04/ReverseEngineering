# Midterm 1
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 2/25/25



## Midterm Math 1
For this challenge, we were given a binary that performed a math calculation on a randomly generated radius. The verify() function would take in the random radius and calculate the area of a circle (Area = pi * radius ^2 ). Using a simple python script and pwn tools, we can use the random radius to calculate the correct area and send it back into the program.

![Screenshot 2025-02-25 221750](https://github.com/user-attachments/assets/9cfe3dee-6cd6-4756-b40c-4fdca34310d6)

Running this script, I got the flag: flag{R0und_R0b1n_0x1123f}
 

## Midterm GDB CountDown 3
For this challenge, we were given access to a service running GDB with 3 phases. The first phase (CountDown 3). In the assembly, you can see that 5 cmp functions occur. In the first one, the program checks to see if the input has a length of 4. If it does not, the execution jumps to an exit and the string does not pass. If it does have a length of 4, it continues. The code the compares each character of user input hex values. The first compares the first character of user input with 0x31 (1 in ascii). The second compares the second character of user input with 0x33 (3 in ascii). This same comparison continues for the 3rd character of user input. Finally the last character of user input is compared with 0x37 (7 in ascii). Therefore, the password is 1337. Inputing that password revealed the flag : flag{C0untd0wn_2_V1ctory_0x6b6d3}

![Screenshot 2025-02-25 224732](https://github.com/user-attachments/assets/662121b6-d295-4591-97d2-afd6f07819fe)


## Midterm GDB Countdown 2
This challenge was a bit more tricky. Using the layout reg command, I was able to step through the instructions to see what the user input was being compared to/checked against. In the first comparison, the user input was checked against 0x6c or 'l' in ascii. Next, the binary compares the next two characters of user input with the first two characters of rbp-0x80. Using the command x/s $rbp-0x80, we can see that this value is 3389, meaning that the next two characters of the password should be 33. So far we have 'l33'. Moving on, the binary compares the fourth character of user input with rbp-0x73. Using the same command as before, we can examine this register and see it's value og 't'. So far the user input should be 'l33t'. Moving on, the program compares user input with rbp-0x60. Sometimes however, it uses subtraction instead of a cmp function. If the result of the subtraction is 0, we know the values match. Here, rbp-0x60 holds the value of 'code'. Our current password is 'l33tcode'. Continuing from here, there is one more condition to be met before the string is 'passed'. The next characters of user input are compared with 'rem'. So, our password currently is 'l33tcoderem'. If the next characters of the user input match 'rem', countdownComplete is called and the string passes. In testing, I inputted this password ("l33tcoderem') and received the flag:  flag{C0untd0wn_2_V1ctory_0xc955}


## Midterm GDB Countdown 1

For this challenge, the program reads in user input to determine the amount of colons, semi-colons, and periods within the user input. The code goes through each character in the user input. If the current character matches a colon, semi-colon, or period, the counter for each is updated. Once all characters of the input have been read, the program checks to see if exactly 3 colons, 2 semi-colons, and 1 period exist within the user input. If so, the string passes. I used layout reg command as well as the x/s command for this challenge. For testing, I inputted "a:b:c:d;f;e." This returned the flag:  flag{C0untd0wn_2_V1ctory_0x6449a}



## Conclusion
Overall I enjoyed these challenges. I did struggle with Phase 2 of the GDB challenge because I forgot about the layout reg command, so I was breaking things down by hand. But once I started to use that command things were a lot easier to analyze.
