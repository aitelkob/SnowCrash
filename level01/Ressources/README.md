
## --------------> Vulnerability

Weak hashed password

Our home directory is once again empty.
Given the scarcity of information, we can consult /etc/passwd, which contains useful information on groups and users.
That said, /etc/passwd remains a popular 'flag' for security analysts and hackers because it's a traditional "hey, I got what I shouldn't" file. If you can read that, you can read other things under /etc as well

# command for searching 
```
>level01@SnowCrash:~$ cat /etc/passwd 
# Output

flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
flag02:x:3002:3002::/home/flag/flag02:/bin/bash
flag03:x:3003:3003::/home/flag/flag03:/bin/bash
flag04:x:3004:3004::/home/flag/flag04:/bin/bash

```
When we display the contents of these /etc/passwd, 
we get what appears to be a A shadow password.

we brute force passwd file with jhon ripper free password cracking software tool

```
~/Desktop ❯ john passwd
Warning: detected hash type "descrypt", but the string is also recognized as "descrypt-opencl"
Use the "--format=descrypt-opencl" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2])
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Warning: Only 9 candidates buffered for the current salt, minimum 128 needed for performance.
Proceeding with wordlist:/Users/yait-el-/.brew/Cellar/john-jumbo/1.9.0/share/john/password.lst, rules:Wordlist
abcdefg          (flag01)
1g 0:00:00:00 DONE 2/3 (2022-04-17 12:17) 50.00g/s 71300p/s 71300c/s 71300C/s raquel..bigman
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```
So we can log in as flag01 with the password "abcdefg" and use getflag to retrieve the token for level01.
