<b id='t'></b>

# :jack_o_lantern:Linux基础命令 #

***

:arrow_double_down:[系统常用命令](#a1)

:arrow_double_down:[CentOS常用命令](#a2)

:arrow_double_down:[日期时间命令](#a3)

:arrow_double_down:[文档打包命令](#a4)

:arrow_double_down:[软件安装命令](#a5)

:arrow_double_down:[网络常用命令](#a6)

<b id='a1'></b>

### :one:系统常用命令 ###

:arrow_double_up:[返回目录](#t)

**关闭系统**

>shutdown now           #立即关机

>shutdown+2             #2min后关机

>shutdown-r now         #重启计算机

>shutdown-h 17：00      #下午5点关机并关闭电源

重启

>reboot

**账号切换**

`（普通用户提示符是$，root是#）`

>su[account]   临时拥有root的权限，不改变当前工作目录以及HOME，SHELL，USER，LOGNAME

>su-[account]  临时拥有root的权限改变当前工作目录以及HOME，SHELL，USER，LOGNAME，PATH变量

示例：

>$su-变更为root  ，su student 变成为student用户

>ctrl+D，exit：退出临时账户

**显示当前目录**

>#pwd 

**查看某个命令所在位置**

这个是指指令所存在的文件夹位置，参数为指令

#whereis[COMMAND]       

示例：

>#whereis pwd

>#whereis ls

**时间显示**

当前时间：

>#date

日历

>#cal

**查看某个文件或目录占用磁盘空间的大小**

>#du

选项：

* h：以人类可读的方式显示（大小显示为kMGTP）
* a：显示全部的档案系统和各分割区的磁盘使用情形
* i：显示i-nodes的使用量
* k：大小用k来表示（默认值）
* t：显示某一个档案系统的所有分割区磁盘使用量
* x：显示不是某一个档案系统的所有分割区磁盘使用量
* T：显示每个分割区所属的档案系统名称

如下：

![](https://github.com/Lumnca/Linux/blob/master/Img/a3.png)

<b id='a2'></b>

### :two:CentOS常用命令 ###

:arrow_double_up:[返回目录](#t)

**列出当前目录中的文件和子目录**

ls[选项][目录名]

选项:

* -a：列出指定目录下的所有内容，包括隐藏文件
* -F：分别用`”/*@|=”`来标记目录，符号链接，管道，socket文件
* -i：输出文件的磁盘索引节点的编号
* -l：用长格式列出文件的各种详细信息（读写操作，权限，日期等信息）

示例

>#ls             #列出当前文件夹内容
>#ll             #等同于ls -l
>#ls/etc/g*      #列出etc文件夹下以g开头的文件
>#ls-al          #用长格式列出文件详细信息（包括隐藏文件）
>#ls-al/etc      #列出/etc目录的内容

**切换到其他目录**

cd[目录名]

示例:

>#cd/home     #进入到/home目录
>#cd..        #返回上一级目录
>#cd          #返回主目录，如jkx用户的主目录默认为：/home/jks,命令等同于cd ~

`. `  :隐藏目录，表示当前目录

`.. `  :隐藏目录，表示上级目录

**创建目录**

mkdir[选项][目录名] 

选项:

* -m：对新建目录设定权限
* -p：按路径自动建立多级目录
* -v：每次创建目录都提示信息

示例

>#mkdir t1                   #在当前位置创建目录t1

>#mkdir-p t1/t12/t123        #在tl/t12创建目录t123

注意这个命令是创建文件夹，而不是文件。

**删除文件或目录**

rmdir[选项][目录名]

选项:

* -p：递归删除目录，当子目录删除后，父目录为空时，父目录也一同删除 *注意：只能删除空目录


示例

>#rmdir t1/t12/t123   #删除位于t1/t12的t123目录(空目录)

>#rmdir-p t1          #删除目录t1(空目录)

**创建新文件（空文件）**

>touch filename

**复制目录或文件**

cp[option][源文件或目录][目标文件或目录]

源文件和目标文件都必须存在！

* -i：提示确认
* -r：递归复制整个目录树、子目录及其它，目标文件必须是目录名
* -v：详细显示文件的复制进度

示例

>#cp datal.txt data2.txt          #将datal.txt复制成data2.txt

>#cp data/     tmp/data           #将data目录复制到/tmp/data目录中

**移动目录或文件-重命名**

mv[option][源文件或目录位置][目标文件或目录]

重命名文件：

>#mv/home/tguo/a/   home/tguo/b

移动文件:

>#mv/home/tguo/a    /root

**删除文件**

rm[option]filename

选项：

* -i:interactive确认删除，避免误删文件。
* -f:force强制删除，不提示
* -v:view显示文件的删除进度。
* -r:recursion 递归删除，可用于非空删除目录

示例:

>#rm -rf dirname         #递归删除目录

>#rm filename

>#rm -f filenamea

>#rm-ir ~/course

**链接**

在Linux的文件系统中，保存在磁盘分区中的文件不管是什么类型都给它分配一个编号，称为索引节点号（Tnode Index）

* 1、硬连接（Hard link）：

>多个文件名指向同一索引节点，这种连接就是硬连接作用：允许一个文件拥有多个有效路径名，以防止“误删”
只有当所有硬连接被删除后，文件的数据块及目录的连接才会被释放

* 2、软连接（Soft link）：

>软链接文件即快捷方式,它是一个特殊的文件，其中包含另一文件的位置信息

**建立链接文件**

In [loptions]source link-name

示例:

在/root 下为文件yy生成一个访问的快捷方式：zz

>#1n-s yy/root/zz        #-s：表示软链接若软链接和源文件不在同一目录，则源文件需要使用绝对路径

>#ln-s/var/www/html/test.css          /root/linktest        

为文件yy产生一个hard link:zz(硬链接)

>#ln yy xx

**查看或者合并文件**

cat[选项][文件名]

cat作用：

* 1.显示文件内容

>#cat filel

2.从键盘创建一个文件

>#cat > file1      #只能创建新文件，不能编辑已有文件[Ctrl]+D退出创建

3.将几个文件合并为一个文件：

>#cat filel file2 > file3

4.向已有文件追加内容

>#cat“aabb” >> file3

5.重定向输入已有文件

>#cat>filel<<EOF

**管道 |（pipes）*

把前一个命令的标准输出作为下一个命令的标准输入.

示例:

>man  cat | more          #查看文件内容，多于一屏则暂停

>#l1 | grep "drw"         #把搜索到文件列表过滤，包含drw将显示出来

># ls /etc | grep 'sys'   #搜索etc目录下含有sys的文件，如下

![](https://github.com/Lumnca/Linux/blob/master/Img/a4.png)

**（管道）参数传递命令**

将前一个命令的标准输出作管道后一个命令的参数示例

>#echo "--help" | cat          #等价于cat  "--help"

>#echo "--help" | xargs cat    #等价于cat  --help

>如果在命令行只输入cat，cat会等待标准输入，这时通过键盘输入并按回车让cat读取输入，cat会原样返回，如输出：--help

>如果在命令行输入cat，同时输入--help并按回车，则会打印帮助

使用这个前提是命令含有参数选项才行，如下为显示var文件夹下的文件：

![](https://github.com/Lumnca/Linux/blob/master/Img/a5.png)

**Xargs总结**

* 不加xargs： 先将管道后面的命令回车执行再从键盘里输入管道符前面命令执行的结果

* 加上xargs： 先从键盘输入管道符前面命令执行的结果再回车,回车的先后顺序不一样

* 实际运用时和find命令配合较多

**分页显示**

more[文件名]

* “空格键”、“Page Down”：查看下一页
* “b”键、“Page Up”：反向查看上一页
* “q”键：退出查看
* 使用方向键查看，可以随意前后浏览文件示例
 
>#more filel.txt    #查看文件内容

>#man ls | more     #查看ls的使用手册

**查看文件中的行数、字数和字符数**

wc[文件名]

* -l统计文件中的行数
* -c统计文件中的字符个数
* -w统计文件中的单词个数

示例：

>#grep ‘dhcp' /var/log/messages | wc -l  #统计系统日志messages中包含dhcp字符串的行数

>#man ls | grep 'ls' | wc -l            #统计ls手册中含有ls的行数

**显示文件的详细信息**

格式

file[文件名]

>file a.txt

查看文件的开头部分

>#head testl.txt

查看文件的结尾部分

>#tail testl.txt

示例：

查看系统日志，服务报错时特别有用

>#tail -f    /var/log/messages    #循环显示最新添加的内容

>#ctrl+c退出

>#tail-20/var/log/messages        #查看日志最后20行

**回显字符串内容**

echo回显内容

示例

* 与重定向符结合

>#echo “hello” >test       #创建文件test并添加内容hello,存在会覆盖原有类容

>#echo “hello” >>test      #向已有的文件test中增加内容hello

**比较两个文件内容的不同**

diff[选项][源文件][目标文件]

选项：

* -a：将所有的文件当作文本文件处理
* -b：忽略空格造成的不同
* -B：忽略空行造成的不同
* -i：忽略大小写的变化

示例

>#diff file1.txt file2.txt

**alias（别名）-重启失效**

格式：alias alias_name="command-definition"

可以为一些常用的命令设置别名，方便调用

示例：

>#alias psa="/bin/ps -aux"       #方便查看进程

>#alias f="find/ -name"          #方便全盘查找
 
>#alias file='cat /etc/sysconfig/network-scripts/ifcfg-ens33'   #查看网络配置文件内容

**alias-永久生效**

* 配置文件：/etc/bashrc
* 每一个运行bash shell的用户都会执行此文件
* 当bash shell被打开时，该文件被读取

设置步骤：

>#vim /etc/bashrc

在最后加入alias alias_name="command-definition“

重启生效

**通配符**

* `*` ：通配符，代表任意字符（0到多个）

* ？：通配符，代表一个字符

示例：

`#ls  *test*     #*表示后面不论接几个字符都接受（没有字符也接受）`
`#ls test？      #？表示后面当且仅当接一个字符时才接受`
`#ls test？？？  #？？？表示一定要接三个字符`

**查找工具find**

`find path -option |-print][-exec-ok command{}\；]`

·path：要搜索文件的目录，若省略，则使用当前目录进行搜索

* -option：用来控制搜索方式

* -print：将搜索结构输出到标准输出[缺省设置]

* -exec command {} \；对查找的结果进行指定的操作{}和\；之间有空格

* -ok：和-exec的作用相同，只不过在操作前要询用户

示例

```
$find ~ -name"*.txt" -print              #在$HOME中查.txt文件并显示
$find . -name “[A-Z]*” -print            #在当前目录查找所有以大写字母开头的文件
$find . -size +1000000c -print           #查长度大于1Mb的文件
$find /etc-name "passwd*" -exec grep "cnscn"f\；

#看是否存在cnscn用户
find/home-mtime-2/home下最近两天内改动过的文件find/home-atime-1查1天之内被存取过的文件find/-amin-10查找在系统中最后10分钟访问的文件
```

如下是寻找/etc中含有sys字符串开头的所有文件：

![](https://github.com/Lumnca/Linux/blob/master/Img/a6.png)

**find和xargs**

如何查找/root下所有包含字符串“hello”的文件？

>#find/root | grep "hello"

>#find/root | xargs grep "hello"

如下是查看website文件的大小：

![](https://github.com/Lumnca/Linux/blob/master/Img/a7.png)

<b id='a3'></b>

### :three:日期时间命令 ###

:arrow_double_up:[返回目录](#t)

**显示指定的月份或者年**

cal[-smjy13][Imonth]year]

* -m：星期一是每周的第一天
* -s：星期日是每周的第一天
* -j：距离本年度1月1日的天数

示例

>#cal           #显示当前月份的日历

>#cal 5 2015    #显示2015年5月的日历

>#cal 2016      #显示2016年的日历

>#cal -y        #显示当年的日历


<b id='a4'></b>

### :three:文档打包命令 ###

:arrow_double_up:[返回目录](#t)

**打包**

打包是指将一大堆文件或目录变成一个总的文件，即归档文件

归档文件（archive file）：为文件或目录备份，建立归档文件

Linux中是常利用tar命令为文件和目录创建归档文件

压缩是将一个大的文件通过压缩算法变成一个小文件

如bz，gzip，zip

**tar命令的使用**

格式：tar[参数][可选项][归档文件名][源文件]

命令功能：用来压缩和解压文件

tar本身不具有压缩功能，它通过调用其它压缩工具实现

常用参数：

* -c建立新的压缩文件        #compress
* -x从压缩的文件中提取文件  #extract
* -v显示操作过程
* -f指定压缩文件    #file
* -t显示压缩文件的内容
* -z是针对gzip
* -j是针对bzip2

示例

* 解包：`#tar xvf FileName.tar`
* 打包：`#tar cvf FileName.tar DirName`

* 单个文件压缩打包 `tar czvf my.tar.gz file1`        #命名为my 打包的是file1
* 多个文件压缩打包 `tar czvf my.tar.gz file1 file2..（file*）（也可以将file*文件mv到一个目录再压缩）`
* 单个目录压缩打包 `tar czvf my.tar.gz dir1`
* 多个目录压缩打包 `tar czvf my.tar.gz dirl dir2`
* 解包至当前目录： `tar xzvf my.tar.gz`


**zip/unzip**

格式：

* 压缩：zip[参数][压缩文件名][源文件]
* 解压：unzip[参数][压缩文件名]

* 优点：跨平台
* 缺点：压缩率不高

>#zip-r archive_name.zip directory_to_compress
>#unzip archive_name.zip

没有该命令可以下载：

`yum install zip`

**gzip**

压缩时不会占用太多CPU，可以得到一个较理想的压缩率,推荐压缩格式

命令格式：

gzip[参数][文件或者目录]

压缩打包文件：`#gzip test.tar`   #压缩产生test.tar.gz,对tar包压缩处理

解压：`#gzip-d archive_name.gz`

如下：

![](https://github.com/Lumnca/Linux/blob/master/Img/a8.png)

<b id='a5'></b>

### :one:软件安装命令 ###

:arrow_double_up:[返回目录](#t)

我们这里只使用yum作为安装命令

**yum**

全面：Yellow dog Updater，Modified

* yum是在Fedora和RedHat以及SUSE中的Shell前端软件包管理器
* yum基于RPM包管理
* yum能够从指定的服务器自动下载RPM包并且安装
* yum可以自动处理依赖性关系，并且一次安装所有依赖的软体包
* 无须像rpm繁琐地一次次下载、安装


命令：yum[选项][命令][安装包名称]

常用选项：

* -h帮助
* -y当安装过程提示选择全部为“yes”
* -q不显示安装的过程

示例：

>yum install             #全部安装

>yum install package1    #安装指定的安装包package1

>yum groupinsall group1  #安装程序组group1

>yum install httpd       #安装Apache服务器

**更新和升级**

```
#yum update全部更新
#yum update package1更新指定程序包package1
#yum check-update检查可更新的程序
#yum upgrade package1升级指定程序包package1
#yum groupupdate group1升级程序组group1
```

**查找和显示**

```
#yum info package1       显示安装包信息package1
#yum list                显示所有已经安装和可以安装的程序包
#yum list package1       显示指定程序包安装情况package1
#yum search string       根据关键字string查找安装包
#yum list I grep string
#yum provides
#yum groupllist/install/removel[tab]
```

**卸载程序**

```
#yum remove package1        删除程序包package1
#yum erase package1         删除程序包package1
#yum groupremove group1     删除程序组group1
#yum deplist package1       查看程序package1依赖情况
```

**清除缓存**

```
#yum clean packages      清除缓存目录下的软件包
#yum clean headers       清除缓存目录下的headers 
#yum clean oldheaders    清除缓存目录下旧的headers 
#yum clean，yum clean all 
#yum clean packages 
#yum clean oldheaders    清除缓存目录下的软件包及旧的headers
```

<b id='a6'></b>

### :one:网络常用命令 ###

:arrow_double_up:[返回目录](#t)

**查看ip地址**

ip[选项]操作对象

示例：

>#ip addr          #显示网卡IP信息

>#ip route list    #查看路由信息

查看网络连通性 :`ping[参数][目标主机或IP地址]`

联网状态 : `netstat[-acCeFghilMnNoprstuvVwx][-A<网络类型>][--ip]`

查看或临时修改主机名 : `hostname[主机名]`

查看或设置路由表 : `route[-n]` -n将路由中的地址信息显示为数字形式

查看当前主机到目的主机经过的节点 : `traceroute[参数-dFlnrvx][目的主机名或ip、域名][数据包大小]`

















