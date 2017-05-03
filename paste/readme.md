paste单词意思是粘贴。该命令主要用来将多个文件的内容合并，与cut命令完成的功能刚好相反。

粘贴两个不同来源的数据时，首先需将其分类，并确保两个文件行数相同。paste将按行将不同文件行信息放在一行。缺省情况下， paste连接时，用空格或tab键分隔新行中不同文本，除非指定-d选项，它将成为域分隔符。

    paste格式为:

    paste -d -s -file1 file2

    选项含义如下：

    -d 指定不同于空格或tab键的域分隔符。例如用@分隔域，使用- d @。

    -s 将每个文件合并成行而不是按行粘贴。
    
    ```
    [zookeeper@master rh]$ cat pas1
ID123
ID345
ID456
ID789
[zookeeper@master rh]$ cat pas2
come
back
hello
world
[zookeeper@master rh]$ paste pas1 pas2
ID123    come
ID345    back
ID456    hello
ID789    world
[zookeeper@master rh]$ paste pas2 pas1
come    ID123
back    ID345
hello    ID456
world    ID789
[zookeeper@master rh]$ paste -d: pas1 pas2　　　　--要创建不同于空格或tab键的域分隔符，使用-d选项。
ID123:come
ID345:back
ID456:hello
ID789:world
[zookeeper@master rh]$ paste -s pas1 pas2　　　　--要合并两行，而不是按行粘贴，可以使用-s选项。
ID123    ID345    ID456    ID789
come    back    hello    world
    ```
    
paste命令还有一个很有用的选项（-）。意即对每一个（-），从标准输入中读一次数据。使用空格作域分隔符，以一个6列格式显示目录列表。方法如下：

```
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5|paste -d@ - - -
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon
adm:3:4:adm@lp:4:7:lp@
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5|paste -d@ - - - 
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon
adm:3:4:adm@lp:4:7:lp@
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5|paste -d@ - - - -
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon@adm:3:4:adm
lp:4:7:lp@@@
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5|paste -d@ - - - - -
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon@adm:3:4:adm@lp:4:7:lp
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5|paste -d@ - - - - - -
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon@adm:3:4:adm@lp:4:7:lp@
[root@master etc]# cat /etc/passwd|cut -d : -f 1,3-5|paste -d@ - - - - - -
root:0:0:root@bin:1:1:bin@daemon:2:2:daemon@adm:3:4:adm@lp:4:7:lp@sync:5:0:sync
shutdown:6:0:shutdown@halt:7:0:halt@mail:8:12:mail@uucp:10:14:uucp@operator:11:0:operator@games:12:100:games
gopher:13:30:gopher@ftp:14:50:FTP User@nobody:99:99:Nobody@dbus:81:81:System message bus@usbmuxd:113:113:usbmuxd user@avahi-autoipd:170:170:Avahi IPv4LL Stack
vcsa:69:69:virtual console memory owner@rtkit:499:497:RealtimeKit@abrt:173:173:@haldaemon:68:68:HAL daemon@saslauth:498:76:"Saslauthd user"@postfix:89:89:
ntp:38:38:@apache:48:48:Apache@avahi:70:70:Avahi mDNS/DNS-SD Stack@pulse:497:496:PulseAudio System Daemon@gdm:42:42:@sshd:74:74:Privilege-separated SSH
tcpdump:72:72:@zookeeper:500:500:zookeeper@hadoop:501:501:@@@
```
