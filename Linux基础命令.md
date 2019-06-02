# :jack_o_lantern:Linux基础命令 #

### :one:系统常用命令 ###

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



### :two:目录常用命令 ###

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











