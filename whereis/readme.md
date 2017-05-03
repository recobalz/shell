　　和find相比，whereis查找的速度非常快，这是因为linux系统会将 系统内的所有文件都记录在一个数据库文件中，当使用whereis和下面即将介绍的locate时，会从数据库中查找数据，而不是像find命令那样，通 过遍历硬盘来查找，效率自然会很高。 

　　但是该数据库文件并不是实时更新，默认情况下时一星期更新一次，因此，我们在用whereis和locate 查找文件时，有时会找到已经被删除的数据，或者刚刚建立文件，却无法查找到，原因就是因为数据库文件没有被更新。 

2.1．命令格式：

whereis [-bmsu] [BMS 目录名 -f ] 文件名

2.2．命令功能：

whereis命令是定位可执行文件、源代码文件、帮助文件在文件系统中的位置。这些文件的属性应属于原始代码，二进制文件，或是帮助文件。whereis 程序还具有搜索源代码、指定备用搜索路径和搜索不寻常项的能力。

2.3．命令参数：

-b   定位可执行文件。

-m   定位帮助文件。

-s   定位源代码文件。

-u   搜索默认路径下除可执行文件、源代码文件、帮助文件以外的其它文件。

-B   指定搜索可执行文件的路径。

-M   指定搜索帮助文件的路径。

-S   指定搜索源代码文件的路径。

2.4．使用实例：

实例1：将和**文件相关的文件都查找出来
```
[finance@master2-dev ~]$ whereis java
java: /usr/bin/java /etc/java /usr/lib/java /usr/share/java /usr/share/man/man1/java.1.gz
[finance@master2-dev ~]$ whereis svn
svn: /usr/bin/svn /usr/share/man/man1/svn.1.gz
[finance@master2-dev ~]$ whereis tomcat
tomcat:
```
tomcat没安装，找不出来，svn安装找出了很多相关文件

实例2：只将二进制文件 查找出来 
```
[finance@master2-dev ~]$ whereis -b svn
svn: /usr/bin/svn
[finance@master2-dev ~]$ whereis -m svn
svn: /usr/share/man/man1/svn.1.gz
[finance@master2-dev ~]$ whereis -s svn
svn:
```
whereis -m svn 查出说明文档路径，whereis -s svn 找source源文件。
