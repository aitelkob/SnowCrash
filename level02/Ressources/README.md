
## --------------> Vulnerability

password sniffing


Level02.pcap file containing packet data captured over a network can be found in our home directory.
Let's take a closer look at the packet data in (www.cloudshark.org). First, we must obtain the file in local.

`scp -P 4242  level02@10.12.100.24:/home/user/level02/level02.pcap .`
*You will maybe have to change permissions on level02.pcap:*
`chmod 677 level02.pcap`
uploded file then follow the transmission : `Analysis Tools`and `Follow Stream`

You can see it if you scroll down a little bit.
```
Password:
ft_wandr...NDRel.L0L

```
We tried this password but it didn't work.

If you return to cloudshark and track the transmission again,
We can see that the dots are represented by [7f] and 7f in the ascii-table is DEL, 

so we must delete one previous character par 7f.

```ft_waNDReL0L ```  is the password
we can log with it to flag02
