# Lab 3: Crack-Me Lab
**Course:** CSE 4830- Software Reverse Engineering  
**Date:** 02/21/25


## Introduction
In class, we have discussed many Libc functions, but not so many that relate to strings and the
manipulation of them.

## Crack-Me 1
For this challenge and each of the following challenges, I used ghidra to statically analyze the binaries and come up with a solution and then tested my solution using the script provided to connect to the service. 

In this challenge, the binary user strcmp() to compare a local variable with the inputted username. Because strcmp() only continues until it comes across a null character, we know that the username must be jerrel for the strcmp to return 0 as the local_58 variable contains "jerrel\0hannah\0matthew\0". Moving on, the binary copies the string "CSE4830_" onto the end of the local_58 variable and then concats the username onto that same string. So at this point, local_58 contains "jerrel\0hannah\0matthew\0CSE4830_jerrel\0". Then, the program compares the inputted password to this "CSE4830_jerrel". If they match, local_60 is set to 1 and the flag is printed!

username: jerrel <br>
password: CSE4830_jerrel <br>
flag: flag{5k1ll_1ssu3_0xcbc4f} <br>

## Crack-Me 2
Again in this challenge, strcmp() is used to check against the local variable local_58 and the inputted username. The first string on the variable before a null character is "alex", which is our username. Moving on, the binary tokenizes the string "this_is_definitely_not_the_password_for_the_lab_today" using the "_" character. Then in a loop, it grabs the 0,1,4,5, and 9 indexed items, resulting in "thisisthepasswordtoday". This string is compared to the inputted password, resulting in the printing of the flag if they match. 

username: alex <br>
password: thisisthepasswordtoday <br>
flag: flag{5k1ll_1ssu3_0xcbc4f} <br>

## Crack-Me 3
Again in this challenge, strcmp() is used to check the username against a local variable containing a variety of strings separated by null characters. In this case, the first string is "louis" and because strcmp() stops comparing when it encounters a null character, we know that this first string must be our username. Moving on, the binary uses strcmp() to compare the inputted password with the first 8 characters of "this-is-also-definitely-not-the-password". We know our password must start with "this-is-". Next, strcmp() is used to check whether the inputted password ends with password, starting after character 17. Finally, it checks to see if the substring of the username exists within the password. So, the password has a template of "this-is-AAAAAAAAApassword" with AAAAAAAAA containing the substring "louis". It is also important to note that the password length must be equal to 25.

username: louis <br>
password: this-is-louisAAAApassword <br>
flag:  flag{5k1ll_1ssu3_0xb360c} <br>


## Crack-Me 4
In this challenge, the local variable was initialized to contain "matthew,hannah,jerrel,chandler,robert,kyle,louis,ian,alex,warren". Then, the binary checked to see if the inputted username was a substring of the local variable. Further, it then checked if the inputted username contained the substring "nd", so I chose the username of "chandler". Moving on, the binary allocated a character buffer and concatenates the first 6 characters of "thisisthepasswordforsure" and the first 5 characters of "maybeitisthis" to the buffer. The buffer now contains "thisismaybe", which is compared to the password. It then uses strcmp() to check whether the end of the password matches "something" - meaning that the password must match "thisismaybesomething". If it does, the flag will be printed.

username: chandler <br>
password: thisismaybesomething <br>
flag: flag{5k1ll_1ssu3_0x8445e} <br>


## Conclusion
Overall I really liked this lab! It forced me to deepen my understanding of strcmp(), strcat(), strcpy(), strstr(), and strtok().
