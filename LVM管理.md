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


**:one:添加硬盘（虚拟机）**

首先要为我们的虚拟机添加硬盘：

![](https://github.com/Lumnca/Linux/blob/master/Img/a14.png)

添加完成后可以输入`#lsblk` 查看硬盘设配：

![](https://github.com/Lumnca/Linux/blob/master/Img/a15.png)

**:two:创建分区**

对新盘分区-root权限

使用fdisk命令对新盘进行分区

```
1、将磁盘/dev/sdb  创建为扩展分区  -extened
2、将扩展分区划分两个逻辑分区，sdb5：2G，sdb6：3G
```

创建步骤如下：

![](https://github.com/Lumnca/Linux/blob/master/Img/a16.png)

我们将硬盘全部分为扩展分区，设置为4999M的原因是5G内存实际上只有4.9G。分完后输入w保存设置。

接下来在再对扩展分区进行分区

![](https://github.com/Lumnca/Linux/blob/master/Img/a17.png)

我将扩展分区分为了2个，一个2G，一个2.9G。接下来再次进入分区类型将其设置为LVM：

![](https://github.com/Lumnca/Linux/blob/master/Img/a18.png)

重复这个操作设置所有分区，保存后输入：fdisk -l，可以看到如下：

![](https://github.com/Lumnca/Linux/blob/master/Img/a19.png)

出现LVM说明操作成功。

**:three:步骤3：创建PV**

使用pvcreate命令创建物理卷，pvs 查看物理卷信息，pvdisplay 查看物理卷详细信息

如下对两个设备进行创建物理卷

![](https://github.com/Lumnca/Linux/blob/master/Img/20.png)

**:four:步骤4：创建VG**

将PV加入VG vgcreate创建新的vg

* vgextend    扩展已有的vg 
* vg          s查询卷组
* vgdisplay   查询卷组详情

`#vgcreate[新vg名称][pv1][pv2]…` 创建新的卷组：

>#vgcreate vgdev /dev/sdb5      将/dev/sdb5分到新建的vgdev组

`#vgextend[已有vg名称][pv1][pv2]…`  扩展已有卷组：

>#vgextend centos /dev/sdb6        将/dev/sdb6作为centos的扩展（即将centos组扩大）

**:five:步骤5：创建LV**

* 1vcreate：命令从卷组里划分一个新的逻辑卷（小于物理卷）
* -L：指定逻辑卷的大小，单位为“kKmMgGtT”字节
* -n：指定逻辑卷名称，默认名称1vol0…

如下：

>#lvcreate -L 960M -n developing vgdev            从vgdev组中创建大小为960M的名为developing的逻辑卷组。

查看逻辑卷组：lvs


**:six:步骤6：使用：格式化逻辑卷并挂载**

新逻辑卷需要经过格式化并挂载到系统里，才能正常使用使用mkfs.xfs格式化为CentOS7的xfs文件系统：

>mkfs.xfs /dev/vgdev/developing        格式化为xfs

再进行挂载：

![](https://github.com/Lumnca/Linux/blob/master/Img/a21.png)

想要永久生效，需要编写/etc/fstab配置文件。将上面得到的UUID写入：

![](https://github.com/Lumnca/Linux/blob/master/Img/a22.png)

***

**用途2：为现有LV扩容**

查看逻辑卷lvs

>#lvextend -L +500M  /dev/vgdemo/Lvodemo1  +500M空间

>#1vextend -L 2G   /dev/vgdemo/1vole   扩展到2G容量

3、在线执行调整（不影响磁盘正常使用），使增加的容量马上使用

* xfs:xfs_growfs
* ext:resize2fs

使修改的逻辑卷生效：`#xfs_growfs /dev/centos/root `  /dev/centos/root为修改的逻辑卷

查看磁盘分区上的可使用的磁盘空间：df-h

不执行在线执行调整，则可使用的磁盘空间不会变化（没加入使用）










