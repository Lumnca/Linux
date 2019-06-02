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

### :two:Centos常用命令 ###

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







