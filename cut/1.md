定义

正如其名，cut的工作就是“剪”，具体的说就是在文件中负责剪切数据用的。cut是以每一行为一个处理对象的，这种机制和sed是一样的

剪切依据

cut命令主要是接受三个定位方法：

第一，字节（bytes），用选项-b

第二，字符（characters），用选项-c

第三，域（fields），用选项-f

语法格式
cut  [-bn] [file] 或 cut [-c] [file]  或  cut [-df] [file]

使用说明
cut 命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段写至标准输出。
如果不指定 File 参数，cut 命令将读取标准输入。必须指定 -b、-c 或 -f 标志之一。

主要参数
-b ：以字节为单位进行分割。这些字节位置将忽略多字节字符边界，除非也指定了 -n 标志。
-c ：以字符为单位进行分割。
-d ：自定义分隔符，默认为制表符。
-f  ：与-d一起使用，指定显示哪个区域。
-n ：取消分割多字节字符。仅和 -b 标志一起使用。如果字符的最后一个字节落在由 -b 标志的 List 参数指示的<br />范围之内，该字符将被写出；否则，该字符将被排除。
```
[root@master etc]# who|cut -b 3-5,8
okee
okee
```
“字节”定位中，提取第3，第4、第5和第8个字节，-b支持形如3-5的写法，而且多个定位之间用逗号隔开

注意，cut命令如果使用了-b选项，那么执行此命令时，cut会先把-b后面所有的定位进行从小到大排序，然后再提取。可不能颠倒定位的顺序哦。
```
[root@master etc]# who|cut -b 9,3-5
oker
oker
```
同时还可以用-3表示从第一个字节到第三个字节，而3-表示从第三个字节到行尾
```
[root@master etc]# who|cut -b -3
zoo
zoo
[root@master etc]# who|cut -b 3-
okeeper pts/0        2016-08-20 20:04 (192.168.184.1)
okeeper pts/2        2016-08-18 19:25 (192.168.184.1)
```
这两种情况下，都是选中第三个字节，同时出现-3,3-也不会出现重复
```
[root@master etc]# who|cut -b 3-,-3
zookeeper pts/0        2016-08-20 20:04 (192.168.184.1)
zookeeper pts/2        2016-08-18 19:25 (192.168.184.1)
```
-b是字节，-c则是字符，注意一点就是：一个空格算一个字节，一个汉字算三个字节

```
[rocrocket@rocrocket programming]$ cat cut_ch.txt
星期一
星期二
星期三
星期四
[rocrocket@rocrocket programming]$ cut -b 3 cut_ch.txt
�
�
�
�
[rocrocket@rocrocket programming]$ cut -c 3 cut_ch.txt
一
二
三
四
[rocrocket@rocrocket programming]$ cat cut_ch.txt |cut -b 2
�
�
�
�
[rocrocket@rocrocket programming]$ cat cut_ch.txt |cut -nb 2　　--当遇到多字节字符时，可以使用-n选项，-n用于告诉cut不要将多字节字符拆开
[rocrocket@rocrocket programming]$ cat cut_ch.txt |cut -nb 1,2,3　　--当遇到多字节字符时，可以使用-n选项，-n用于告诉cut不要将多字节字符拆开
星 
星 
星 
星
```
为什么会有“域”的提取呢，因为刚才提到的-b和-c只能在固定格式的文档中提取信息，而对于非固定格式的信息则束手无策。这时候“域”就派上用场了。如果你观察过/etc/passwd文件，你会发现，它并不像who的输出信息那样具有固定格式，而是比较零散的排放。但是，冒号在这个文件的每一行中都起到了非常重要的作用，冒号用来隔开每一个项。

我们很幸运，cut命令提供了这样的提取方式，具体的说就是设置“间隔符”，再设置“提取第几个域”，就OK了！

```
[root@master etc]# cat /etc/passwd |head -n 5
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1
root
bin
daemon
adm
lp
[root@master etc]# cat /etc/passwd|head -n 5|cut -d : -f 1,3-5
root:0:0:root
bin:1:1:bin
daemon:2:2:daemon
adm:3:4:adm
lp:4:7:lp
```
有时候制表符确实很难辨认，有一个方法可以看出一段空格到底是由若干个空格组成的还是由一个制表符组成的

```
[zookeeper@master rh]$ sed -n l test.txt 
this is first line$
this is second line$
this is third line$
this is fourth line$
this\tfifth line$
happy everyday$
end$
```
如果是制表符（TAB），那么会显示为\t符号，如果是空格，就会原样显示。通过此方法即可以判断制表符和空格了。

这是sed中的用法：l  [n]

用明确的形式显示模版空间的数据：

①、以C-style的转义形式显示不能打印的字符(换行符、制表符等)和本身的\Char形式；

②、长的行将进行分割，以字符\结尾的行表示分割，以字符$结尾的行表示分割结束。

③、n指定显示行的长度，超过就进行分割；若为0表示不分割所有行；没有指定时就取命令行选项-l的设置，再没有就取默认值70。这是GNU的扩展功能。

其实cut的-d选项的默认间隔符就是制表符，所以当你就是要使用制表符的时候，完全就可以省略-d选项，而直接用－f来取域就可以了！如果你设定一个空格为间隔符，使用 -d ' '而且，你只能在-d后面设置一个空格，可不许设置多个空格，因为cut只允许间隔符是一个字符。
