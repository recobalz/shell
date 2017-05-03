我们经常在linux要查找某个文件，但不知道放在哪里了，可以使用下面的一些命令来搜索： 
　　　which  查看可执行文件的位置。
　　　whereis 查看文件的位置。 
　　　locate   配合数据库查看文件位置。
　　　find   实际搜寻硬盘查询文件名称。

1.which 

　　which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。

1.1．命令格式：

which 可执行文件名称 

1.2．命令功能：

which指令会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。

1.3．命令参数：

-n 　指定文件名长度，指定的长度必须大于或等于所有文件中最长的文件名。

-p 　与-n参数相同，但此处的包括了文件的路径。

-w 　指定输出时栏位的宽度。

-V 　显示版本信息

1.4．使用实例：

实例1：查找文件、显示命令路径
```
[finance@master2-dev ~]$ which pwd
/bin/pwd
[finance@master2-dev ~]$ which passwd
/usr/bin/passwd
[finance@master2-dev ~]$ which java
/home/bigdata/software/java/bin/java
```
which 是根据使用者所配置的 PATH 变量内的目录去搜寻可运行档的！所以，不同的 PATH 配置内容所找到的命令当然不一样的！

实例2：用 which 去找出 which
```
[finance@master2-dev ~]$ which which
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
    /usr/bin/which
```
说明：

竟然会有两个 which ，其中一个是 alias 这就是所谓的『命令别名』，意思是输入 which 会等於后面接的那串命令！

实例3：找出 cd 这个命令
```
[finance@master2-dev ~]$ which cd
/usr/bin/which: no cd in (/home/bigdata/software/java/bin:/usr/lib64/qt-3.3/bin:/home/bigdata/software/spark-1.5.2.2-bin-2.4.0.7/bin:/home/bigdata/software/R-3.2.1/bin:/home/bigdata/software/anaconda/bin:/home/bigdata/software/java/bin:/home/bigdata/software/hadoop/bin:/home/bigdata/software/hive/bin:/home/bigdata/software/sqoop/bin:/home/bigdata/software/hbase/bin:/home/bigdata/software/zeppelin/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/bigdata/software/hadoop/bin:/home/bigdata/software/hive/bin:/home/bigdata/software/sqoop/bin:/home/finance/bin)
```
说明：

cd 这个常用的命令竟然找不到啊！为什么呢？这是因为 cd 是bash 内建的命令！ 但是 which 默认是找 PATH 内所规范的目录，所以当然一定找不到的！
