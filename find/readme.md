　Linux下find命令在目录结构中搜索文件，并执行指定的操作。Linux下find命令提供了相当多的查找条件，功能很强大。由于find具有强大的功能，所以它的选项也很多，其中大部分选项都值得我们花时间来了解一下。即使系统中含有网络文件系统( NFS)，find命令在该文件系统中同样有效，只你具有相应的权限。 在运行一个非常消耗资源的find命令时，很多人都倾向于把它放在后台执行，因为遍历一个大的文件系统可能会花费很长的时间

1.1．命令格式：

find pathname -options [-print -exec -ok ...]

1.2．命令功能：

用于在文件树种查找文件，并作出相应的处理

1.3．命令参数：

pathname: find命令所查找的目录路径。例如用.来表示当前目录，用/来表示系统根目录。 
-print： find命令将匹配的文件输出到标准输出。 
-exec： find命令对匹配的文件执行该参数所给出的shell命令。相应命令的形式为'command' {  } \;，注意{   }和\；之间的空格。 
-ok： 和-exec的作用相同，只不过以一种更为安全的模式来执行该参数所给出的shell命令，在执行每一个命令之前，都会给出提示，让用户来确定是否执行。

```
[finance@master2-dev ~]$ cat 123.txt 
qwda
[finance@master2-dev ~]$ find 123* -ok cat {} \;
< cat ... 123.txt > ? yes
qwda
[finance@master2-dev ~]$ find 123* -exec cat {} \;
qwda
[finance@master2-dev ~]$ find 123*| xargs cat
qwda
```
1.4．命令选项：

```
-name   filename             #查找名为filename的文件
-perm                        #按执行权限来查找
-user    username             #按文件属主来查找
-group groupname            #按组来查找
-mtime   -n +n                #按文件更改时间来查找文件，-n指n天以内，+n指n天以前
-atime    -n +n               #按文件访问时间来查GIN: 0px">
-ctime    -n +n              #按文件创建时间来查找文件，-n指n天以内，+n指n天以前
-nogroup                     #查无有效属组的文件，即文件的属组在/etc/groups中不存在
-nouser                     #查无有效属主的文件，即文件的属主在/etc/passwd中不存
-newer   f1 !f2              找文件，-n指n天以内，+n指n天以前 
-ctime    -n +n               #按文件创建时间来查找文件，-n指n天以内，+n指n天以前 
-nogroup                     #查无有效属组的文件，即文件的属组在/etc/groups中不存在
-nouser                      #查无有效属主的文件，即文件的属主在/etc/passwd中不存
-newer   f1 !f2               #查更改时间比f1新但比f2旧的文件
-type    b/d/c/p/l/f         #查是块设备、目录、字符设备、管道、符号链接、普通文件
-size      n[c]               #查长度为n块[或n字节]的文件
-depth                       #使查找在进入子目录前先行查找完本目录
-fstype                     #查更改时间比f1新但比f2旧的文件
-type    b/d/c/p/l/f         #查是块设备、目录、字符设备、管道、符号链接、普通文件
-size      n[c]               #查长度为n块[或n字节]的文件
-depth                       #使查找在进入子目录前先行查找完本目录
-fstype                      #查位于某一类型文件系统中的文件，这些文件系统类型通常可 在/etc/fstab中找到
-mount                       #查文件时不跨越文件系统mount点
-follow                      #如果遇到符号链接文件，就跟踪链接所指的文件
-cpio                %;      #查位于某一类型文件系统中的文件，这些文件系统类型通常可 在/etc/fstab中找到
-mount                       #查文件时不跨越文件系统mount点
-follow                      #如果遇到符号链接文件，就跟踪链接所指的文件
-cpio                        #对匹配的文件使用cpio命令，将他们备份到磁带设备中
-prune                       #忽略某个目录
```
-size n：[c] 查找文件长度为n块的文件，带有c时表示文件长度以字节计。-depth：在查找文件时，首先查找当前目录中的文件，然后再在其子目录中查找。
-fstype：查找位于某一类型文件系统中的文件，这些文件系统类型通常可以在配置文件/etc/fstab中找到，该配置文件中包含了本系统中有关文件系统的信息。
-mount：在查找文件时不跨越文件系统mount点。
-follow：如果find命令遇到符号链接文件，就跟踪至链接所指向的文件。
-cpio：对匹配的文件使用cpio命令，将这些文件备份到磁带设备中。

另外,下面三个的区别:

-amin n   查找系统中最后N分钟访问的文件
-atime n  查找系统中最后n*24小时访问的文件
-cmin n   查找系统中最后N分钟被改变文件状态的文件
-ctime n  查找系统中最后n*24小时被改变文件状态的文件
-mmin n   查找系统中最后N分钟被改变文件数据的文件
-mtime n  查找系统中最后n*24小时被改变文件数据的文件

1.5.实例

1.5.1．使用name选项：

文件名选项是find命令最常用的选项，要么单独使用该选项，要么和其他选项一起使用。  可以使用某种文件名模式来匹配文件，记住要用引号将文件名模式引起来。  不管当前路径是什么，如果想要在自己的根目录$HOME中查找文件名符合*.log的文件，使用~作为 'pathname'参数，波浪号~代表了你的$HOME目录。
```
find ~ -name "*.log" -print  ------*表示  通配任意的字符　　　　　　？表示  通配任意的单个字符
```
想要在当前目录及子目录中查找所有的‘ *.log‘文件，可以用： 
```
find . -name "*.log" -print  
```
想要的当前目录及子目录中查找文件名以一个大写字母开头的文件，可以用：  
```
find . -name "[A-Z]*" -print  ---------[ ] 表示 通配括号里面的任意一个字符
```
想要在/etc目录中查找文件名以host开头的文件，可以用：  
```
find /etc -name "host*" -print  
```
如果想在当前目录查找文件名以一个个小写字母开头，最后是4到9加上.log结束的文件：  
```
[finance@master2-dev ~]$ find . -name "[a-z]*[4-9].log" -print
./script/hs_err_pid32186.log
./script/hs_err_pid19725.log
./script/hs_err_pid5736.log
```
1.5.2．用perm选项：

按照文件权限模式用-perm选项,按文件权限模式来查找文件的话。最好使用八进制的权限表示法。  

如在当前目录下查找文件权限位为755的文件，即文件属主可以读、写、执行，其他用户可以读、执行的文件，可以用：  

[finance@master2-dev ~]$ find . -perm 755 -print
还有一种表达方法：在八进制数字前面要加一个横杠-，表示都匹配，如-007就相当于777，-005相当于555,

1.5.3．忽略某个目录：

如果在查找文件时希望忽略某个目录，因为你知道那个目录中没有你所要查找的文件，那么可以使用-prune选项来指出需要忽略的目录。在使用-prune选项时要当心，因为如果你同时使用了-depth选项，那么-prune选项就会被find命令忽略。如果希望在test目录下查找文件，但不希望在test/test3目录下查找，可以用：  

命令：
```
find test -path "test/test3" -prune -o -print


[finance@master2-dev ~]$ find . -name *log
./nby/dm_rpt_070001_rds_d.log
./nby/rpt_sms_001_d_group.log
./nby/rpt_170006_ams_d.log
./nby/mls_epp_member_lab_info.log
./nby/rpt_170009_ams_d.log
./nby/bi_td.tsor_page_prmtr_enter_td.log
./nby/tmp_act_acct_info.log
./nby/rpt_170010_ams_d.log
./nby/bi_td.tsor_br_page_cate_td.log
./nby/dpa_pty_onl_rgst_info.log
./nby/finance.sor_mls_fnd_bill_order.log
./.sqoop/shared-metastore.db.log
./script/hs_err_pid30353.log
./script/hs_err_pid32186.log
./script/hs_err_pid19725.log
./script/log
./script/log/hiveserver2.log
./script/log/restartHiveServer2.log
./script/hs_err_pid5736.log
./syf/dm_rpt_090005_pcs_m.log
./syf/dm_rpt_090006_pcs_m.log
./syf/dm_rpt_090004_pcs_m.log
[finance@master2-dev ~]$ find . -path "./syf" -prune -o -name *log -print
./nby/dm_rpt_070001_rds_d.log
./nby/rpt_sms_001_d_group.log
./nby/rpt_170006_ams_d.log
./nby/mls_epp_member_lab_info.log
./nby/rpt_170009_ams_d.log
./nby/bi_td.tsor_page_prmtr_enter_td.log
./nby/tmp_act_acct_info.log
./nby/rpt_170010_ams_d.log
./nby/bi_td.tsor_br_page_cate_td.log
./nby/dpa_pty_onl_rgst_info.log
./nby/finance.sor_mls_fnd_bill_order.log
./.sqoop/shared-metastore.db.log
./script/hs_err_pid30353.log
./script/hs_err_pid32186.log
./script/hs_err_pid19725.log
./script/log
./script/log/hiveserver2.log
./script/log/restartHiveServer2.log
./script/hs_err_pid5736.log
```
实例2：避开多个文件夹:

```
[finance@master2-dev ~]$ find .  \(  -path "./syf" -o -path "./script" \)  -prune -o -name *log -print
./nby/dm_rpt_070001_rds_d.log
./nby/rpt_sms_001_d_group.log
./nby/rpt_170006_ams_d.log
./nby/mls_epp_member_lab_info.log
./nby/rpt_170009_ams_d.log
./nby/bi_td.tsor_page_prmtr_enter_td.log
./nby/tmp_act_acct_info.log
./nby/rpt_170010_ams_d.log
./nby/bi_td.tsor_br_page_cate_td.log
./nby/dpa_pty_onl_rgst_info.log
./nby/finance.sor_mls_fnd_bill_order.log
./.sqoop/shared-metastore.db.log
```
说明：

圆括号表示表达式的结合。  \ 表示引用，即指示 shell 不对后面的字符作特殊解释，而留给 find 命令去解释其意义。  

1.5.4．使用user和nouser选项：

按文件属主查找文件：

实例1：在$HOME目录中查找文件属主为finance的文件 

命令：
```
find ~ -user finance -print
```
实例2：在/etc目录下查找文件属主为peida的文件: 

命令：
```
find /etc/ -user finance -print
```
说明：

实例3：为了查找属主帐户已经被删除的文件，可以使用-nouser选项。在/home目录下查找所有的这类文件

命令：
```
find /home -nouser -print
```
说明：

这样就能够找到那些属主在/etc/passwd文件中没有有效帐户的文件。在使用-nouser选项时，不必给出用户名； find命令能够为你完成相应的工作。

1.5.5．使用group和nogroup选项：

就像user和nouser选项一样，针对文件所属于的用户组， find命令也具有同样的选项，为了在/home/finance目录下查找属于gem用户组的文件，可以用：  
```
find /home/finance/ -group finance -print  
```
要查找没有有效所属用户组的所有文件，可以使用nogroup选项。下面的find命令从文件系统的根目录处查找这样的文件:
```
find / -nogroup-print
```
1.5.6．按照更改时间或访问时间等查找文件：

如果希望按照更改时间来查找文件，可以使用mtime,atime或ctime选项。如果系统突然没有可用空间了，很有可能某一个文件的长度在此期间增长迅速，这时就可以用mtime选项来查找这样的文件。  

用减号-来限定更改时间在距今n日以内的文件，而用加号+来限定更改时间在距今n日以前的文件。  

希望在系统/home/finance/下查找更改时间在5日以内的文件，可以用：

```
[finance@master2-dev ~]$ find /home/finance/ -mtime -5 -print
/home/finance/
/home/finance/123.txt
/home/finance/.bash_history
/home/finance/data.txt
/home/finance/.viminfo
/home/finance/.subversion
/home/finance/.subversion/README.txt
/home/finance/.subversion/servers
/home/finance/.subversion/auth
/home/finance/.subversion/auth/svn.simple
/home/finance/.subversion/auth/svn.username
/home/finance/.subversion/auth/svn.ssl.server
/home/finance/.subversion/auth/svn.ssl.client-passphrase
/home/finance/.subversion/config
/home/finance/script
/home/finance/script/monitor_hiveserver2_leak.sh
/home/finance/script/log/hiveserver2.log
```
1.5.7．查找比某个文件新或旧的文件：

如果希望查找更改时间比某个文件新但比另一个文件旧的所有文件，可以使用-newer选项。

它的一般形式为：  

newest_file_name ! oldest_file_name  

其中，！是逻辑非符号。  

```
[finance@master2-dev nby]$ ll
total 500
-rw-r--r-- 1 finance finance 123029 Dec 18  2015 170006.csv
-rw-r--r-- 1 finance finance   4144 Dec 18  2015 170007.csv
-rw-r--r-- 1 finance finance  21075 Dec 18  2015 170009.csv
-rw-r--r-- 1 finance finance 135309 Dec 18  2015 170010.csv
-rw-rw-r-- 1 finance finance   6811 Jun 16  2015 bi_td.tsor_br_page_cate_td.log
-rw-rw-r-- 1 finance finance   6842 Jun 16  2015 bi_td.tsor_page_prmtr_enter_td.log
-rw-r--r-- 1 finance finance    129 Apr 21  2015 bsn.txt
-rw-rw-r-- 1 finance finance  22942 Apr 22  2015 dm_rpt_070001_rds_d.log
-rw-rw-r-- 1 finance finance  25510 Aug 18  2015 dpa_pty_onl_rgst_info.log
-rw-rw-r-- 1 finance finance    167 Apr 21  2015 finance.csv
-rw-rw-r-- 1 finance finance    751 Apr  1  2015 finance.sor_mls_fnd_bill_order.log
-rw-rw-r-- 1 finance finance    126 Apr 21  2015 finance.txt
-rw-rw-r-- 1 finance finance   5326 Jun 10  2015 mls_epp_member_lab_info.log
-rw-r--r-- 1 finance finance     30 Apr 21  2015 nat.txt
-rw-rw-r-- 1 finance finance  24983 Dec 18  2015 rpt_170006_ams_d.log
-rw-rw-r-- 1 finance finance  24424 Dec 18  2015 rpt_170009_ams_d.log
-rw-rw-r-- 1 finance finance  24495 Dec 18  2015 rpt_170010_ams_d.log
-rw-rw-r-- 1 finance finance    513 Jun 10  2015 rpt_sms_001_d_group.log
-rw-r--r-- 1 finance finance     83 Apr 21  2015 rule.txt
-rw-rw-r-- 1 finance finance  24856 Aug 18  2015 tmp_act_acct_info.log
[finance@master2-dev nby]$ find -newer bi_td.tsor_br_page_cate_td.log
.
./170006.csv
./rpt_170006_ams_d.log
./170009.csv
./170010.csv
./rpt_170009_ams_d.log
./170007.csv
./bi_td.tsor_page_prmtr_enter_td.log
./tmp_act_acct_info.log
./rpt_170010_ams_d.log
./dpa_pty_onl_rgst_info.log
[finance@master2-dev nby]$ find ! -newer bi_td.tsor_br_page_cate_td.log
./dm_rpt_070001_rds_d.log
./rpt_sms_001_d_group.log
./finance.csv
./mls_epp_member_lab_info.log
./bsn.txt
./finance.txt
./nat.txt
./bi_td.tsor_br_page_cate_td.log
./rule.txt
./finance.sor_mls_fnd_bill_order.log
[finance@master2-dev nby]$ find -newer bi_td.tsor_br_page_cate_td.log ! -newer bi_td.tsor_page_prmtr_enter_td.log
./bi_td.tsor_page_prmtr_enter_td.log
[finance@master2-dev nby]$ find -newer bi_td.tsor_br_page_cate_td.log ! -newer 170006.csv 
./170006.csv
./170009.csv
./170010.csv
./bi_td.tsor_page_prmtr_enter_td.log
./tmp_act_acct_info.log
./dpa_pty_onl_rgst_info.log
```
1.5.8．使用type选项：

实例1：在/etc目录下查找所有的目录  

命令：

```
[finance@master2-dev ~]$ find  -type d
.
./20160427
./export_temp_dpa_rist_cust_init.txt
./fanghh
./fanghh/lx
./fanghh/script
./nby
./jars
./.gnome2
./.gnome2/keyrings
./.sqoop
./.subversion
./.subversion/auth
./.subversion/auth/svn.simple
./.subversion/auth/svn.username
./.subversion/auth/svn.ssl.server
./.subversion/auth/svn.ssl.client-passphrase
./script
./script/log
./.ssh
./syf
./tmp
./mls
```
实例2：在当前目录下查找除目录以外的所有类型的文件  

命令：

```
[finance@master2-dev ~]$ find . ! -type d -print 
./.history
./123.txt
./.bashrc
./export_temp_dpa_rist_cust_init.txt/.000000_0.deflate.crc
./export_temp_dpa_rist_cust_init.txt/000001_0.deflate
./export_temp_dpa_rist_cust_init.txt/.000001_0.deflate.crc
./export_temp_dpa_rist_cust_init.txt/000000_0.deflate
./%s
./fanghh/test.py
./fanghh/rules.py
./fanghh/lx/dm_app_finance_service_daily_data_20160801.csv
./fanghh/script/dm_rpt_130019_fsa_web_02_d.sql
./fanghh/script/dpa_fsa_br_base_pageview.sql
./fanghh/script/dm_rpt_130016_fsa_web_01_d.sql
./fanghh/script/dm_rpt_130014_fsa_web_d.sql
......
```
实例3：在当前目录下查找所有的符号链接文件

命令：
```
[finance@master2-dev ~]$ find -type l -print
./workspace
```
1.5.9．使用size选项：

可以按照文件长度来查找文件，这里所指的文件长度既可以用块（block）来计量，也可以用字节来计量。以字节计量文件长度的表达形式为N c；以块计量文件长度只用数字表示即可。  

在按照文件长度查找文件时，一般使用这种以字节表示的文件长度，在查看文件系统的大小，因为这时使用块来计量更容易转换。  

实例1：在当前目录下查找文件长度大于1 M字节的文件  

命令：
```
[finance@master2-dev ~]$ find . -size +1000000c -print
./fanghh/script/dpa_cde_fsa_br_utm_src.init
./fanghh/hadoop-0.0.1-jar-with-dependencies.jar
./jars/spark-assembly-1.1.0-hadoop2.1.0.jar
./script/log/hiveserver2.log
./tmp/export_dpa_crd_cshop_main_info.txt
```
实例2：在/home/apache目录下查找文件长度恰好为100字节的文件:  

命令：
```
find /home/apache -size 100c -print  
```
实例3：在当前目录下查找长度超过10块的文件（一块等于512字节） 

命令：
```
find . -size +10 -print
```
1.5.10．使用depth选项：

在使用find命令时，可能希望先匹配所有的文件，再在子目录中查找。使用depth选项就可以使find命令这样做。这样做的一个原因就是，当在使用find命令向磁带上备份文件系统时，希望首先备份所有的文件，其次再备份子目录中的文件。  

实例1：find命令从文件系统的根目录开始，查找一个名为CON.FILE的文件。   

命令：
```
find / -name "CON.FILE" -depth -print
```
说明：

它将首先匹配所有的文件然后再进入子目录中查找

1.5.11．使用mount选项： 

  在当前的文件系统中查找文件（不进入其他文件系统），可以使用find命令的mount选项。

实例1：从当前目录开始查找位于本文件系统中文件名以XC结尾的文件  

命令：
```
find . -name "*.XC" -mount -print 
```
 
