# Midterm 2
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 2/27/25



## Midterm Math 2
For this challenge I used Ghidra to analyze the provided binary. We were given a binary that executed a verify() function within a loop with 50 iterations.The program also generated a random number using the rand() function at the start of each iteration . The verify() function took in the random number and the user Input and would return true if the random number divided by the user input equaled the result of the iteration count mod 15 plus one. Each iteration must return true for the flag to be displayed. I wrote a python script to recreate this loop and ensure that the user input satisfied this condition. 

![image](https://github.com/user-attachments/assets/24da0569-0414-4614-97d7-5df3917bbbab)

 flag --> flag{N0t_T00_10ng_0xea572}

## Midterm String
For this challenge, I again used Ghidra to analyze the provided binary. The program contained a login function that took in a username and password from the user. For the flag to be displayed, the login function had to return 0. In the login function, the program took the first two characters of user input and the third character of user input, converted them to decimal values, and then used to values to pull a substring out of a previously defined string. For the flag to be printed, there are a few conditions that need to be met:
1. username must = marcus
2. the sum of the two numbers < 96 (length of previously defined string)
3. the first number must match the index of 's' in 'smelly' - 1
4. the second number must be 6 (the length of 'smelly)

I found that the 'smelly' substring starts at index 46 of the previously defined string ("coolnicefreshwashedspotlessunbecomingdelicatesmellylaughableicydynamicfreecheapaloofroughhungry")

I inputed the password 456. The program grabbed 45 as the first number, and 6 as the second number. Using these two numbers, the program pulls the substring "smelly" out of the predefined string and concats the substring "is". It then performs a string compare with the string "issmelly". If these strings match, the login function returns 0 and the flag is displayed.

username: marcus
password: 456

flag --> flag{S0m3th1ng_Sm3lls_0xdb1f4}


