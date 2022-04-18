
## --------------> Vulnerability

Exploring Weak Ciphers

Our home directory is currently empty.
However, we are given a hint: look for files associated with user flag00.

# command for searching 
```
>level00@SnowCrash:~$ find / -user flag00 2>/dev/null
# Output
/usr/sbin/john
/rofs/usr/sbin/john
```
When we display the contents of these two files, 
we get what appears to be a plain text password.


```
level00@SnowCrash:~$ cat /usr/sbin/jhon
cdiiddwpgswtgt
```


This password isn't working for flag00 or level01. Is it encrypted?
Let's go to dcode.fr to see if any cryptography tools can assist in 
decoding the password.


We can try all rotats by in the tool for Ceasar encryption (commonly known as rot)
(https://www.dcode.fr/caesar-cipher)

When the password is subjected to a 15-character rotation, the result is suspiciously well-worded.

Log in as flag00 with password `nottoohardhere`

