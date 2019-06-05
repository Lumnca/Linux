<b id='t'></b>

# :jack_o_lantern:LVM管理 #

***

:arrow_double_down:[主要内容](#a1)

:arrow_double_down:[LVM添加硬盘以及扩容](#a1)

***

<b id='a1'></b>

### :beginner:主要内容 ###

:arrow_double_up:[返回目录](#t)

**:one:**

>LVM：逻辑卷管理，LVM是Logical Volume Manager（逻辑卷管理）的简写，Linux环境下对磁盘分区进行管理的一种机制，类似windows>>动态磁盘

传统的文件系统-Linux，Windows特点：

```
1、基于分区，一个文件系统对应一个分区
2、不同的分区相对独立，无相互联系-空间容易利用不平衡
3、无法扩充文件系统大小
4、无法动态合并分区-只能合并再分区，需要对所有数据备份与恢复
```

LVM-Linux，Unix特点：

```
1、将多个硬盘当做一个大硬盘使用（逻辑卷）-统一管理
2、动态无损调整分区（文件系统）大小-空间按需分配，不会丢失数据
3、若大硬盘（逻辑卷）空间不够用，还可动态加入其它硬盘-空间无限
```

LVM适用范围 :适用于装有多个磁盘的系统（PC，服务器均可）

**:two:术语**

1、物理存储介质（PhysicalStorageMedia）即磁盘（硬件设备）

```
系统的物理存储设备存储系统最底层的存储单元
```

2、物理卷（Physical Volume，PV）

```
即磁盘分区，或从逻辑上与磁盘分区具有同样功能的设备（如RAID）
基本逻辑存储块
磁盘分区包含有与LVM相关的管理参数
fdisk命令创建
```

3、卷组（Volume Group，VG）

```
即大磁盘
由一个或多个物理卷所组成的存储池（多个磁盘分区组成的集合）在卷组上能创建一个或多个逻辑卷
```

4、逻辑卷（Logical Volume，LV）

```
即逻辑磁盘分区，可动态改变大小类似于非LVM系统中的磁盘分区
逻辑卷建立在卷组VG上
在逻辑卷上可以创建各种文件系统（比如/home或者/usr等）
```

5、物理块（Physical Extent，PE）

```
每一个物理卷PV被划分为称为PE（Physical Extents）的基本单元
具有唯一编号的PE是可以被LVM寻址的最小单元
PE的大小是可配置的，默认为4MB
所有物理卷（PV）由大小等同的基本单元PE组成
```

6、逻辑块（Logical Extent，LE）

```
逻辑卷LV也被划分为可被寻址的基本单位，称为LE
在同一个卷组中，LE的大小和PE是相同的，并且一一对应
```

LVM分层及流程：PV>>VG>>LV

***

<b id='a2'></b>

### :beginner:LVM添加硬盘以及扩容 ###

:arrow_double_up:[返回目录](#t)






























