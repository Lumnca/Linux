<b id='t'></b>

# :jack_o_lantern:Linux文件共享 #

***

:arrow_double_down:[Samba](#a1)

:arrow_double_down:[Samba配置步骤](#a2)

:arrow_double_down:[NTFS配置](#a3)

***

<b id='a1'></b>

### :game_die:Samba ###

:arrow_double_up:[返回目录](#t)

Samba软件

早期名称 SMBServer，因为SMB是没有意义的文字，因此没有办法注册成商标，而Samba（桑巴舞）刚好有意义，就采用此名称注册smb软件

```
Samba是在Linux和UNIX系统上实现SMB协议的一个免费软件
Samba主要用于Unix（Linux）与Windows共享文件
服务端：Unix或Linux
客户端：Windows
```

samba安装: `#yum install samba`

运行 : `#systemctl start smb`

重启：`#systemctl restart smb`

停止 :`#systemctl stop smb`

状态 :`#systemctl status smb`

注意关闭或者设置：防火墙和SELinux.

**:one:samba主配置文件详解**

主配置文件：/etc/samba/smb.conf

```
·smb.conf含有多个段，每个段由段名开始，直到下个段名
·每个段名放在方括号中间
·每段的参数的格式是：名称=指令
·配置文件中一行一个段名和参数，段名和参数名不分大小写。
·除了[global]段外，所有的段都可以看作是一个共享资源
·段名是该共享资源的名字，段里的参数是该共享资源的属性。
```

`testparm `：测试smb.conf配置是否正确

`testparm -v`：详细的列出smb.conf支持的配置参数

全局参数：[global]

`workgroup=WORKGROUP`说明：设定Samba Server所要加入的工作组或者域

`server string=Samba Server Version %v`说明：设定Samba Server的注释，可以是任何字符串，也可以不填。宏%v表示显示Samba的版本号。

`netbios name=smbserver`说明：设置Samba Server的NetBIOS名称，netbios name和workgroup名字不要设置成一样了。

`interfaces=lo eth0 192.168.12.2/24 192.168.13.2/24`说明：设置Samba Server监听哪些网卡，可以写网卡名，也可以写该网卡的IP地址

`hosts allow=192.168.1.*192.168.10.1`说明：表示允许连接到Samba Server的客户端，多个参数以空格隔开。可以用一个IP表示，也可以用一个网段表示。hosts deny与hosts allow刚好相反

`security=user`说明：设置用户访问Samba Server的验证方式，有四种验证方式：

* share：用户访问Samba Server不需要提供用户名和口令，安全性能较低。
* user:Samba Server共享目录只能被授权的用户访问，由Samba Server负责检查账号和密码的正确性。账号和密码要在本Samba Server中建立。
* server：依靠其他Windows NT/2000或Samba Server来验证用户的账号和密码，是一种代理验证。此种安全模式下，系统管理员可以把所有的Windows用户和口令集中到一个NT系统上，使用Windows NT进行Samba认证，远程服务器可以自动认证全部用户和口令，如果认证失败，Samba将使用用户级安全模式作为替代方式。
* domain：域安全级别，使用主域控制器（PDC）来完成认证。

`writable=yes/no` 说明：writable用来指定该共享路径是否可写。

`available=yes/no` 说明：available用来指定该共享资源是否可用。

`valid users=允许访问该共享的用户` 说明：valid users用来指定允许访问该共享资源的用户。

`public=yes/no` 说明：public用来指定该共享是否允许guest账户访问。

`guest ok=yes/no` 说明：意义同“public”。

**:two:设置samba用户密码**

```
系统用户密码和Samba用户的密码是不同的
命令：#smbpasswd
作用：设置Samba用户密码
其原理是通过读取/etc/passwd文件中存在的用户名，并设置密码
示例：
#smbpasswd alice
```

***

<b id='a2'></b>

### :game_die:Samba配置步骤 ###

:arrow_double_up:[返回目录](#t)

假设我们要实现以下需求：

|共享名|路径|权限|
|:--|:---|:-----|
|SHAREDOC|`/smb/all` |所有人员包括来宾均可以访问仅允许特定组的用户进行读写访问|
|RDDOCS | `/smb/user` |如建立一个组RD：包含3个用户：Alice,jack,Tom可以访问user|


**1、建立共享文件夹及文件**：

```
mkdir -p /smb/user
mkdir -p /smb/all

touch /smb/all/file.txt
touch /smb/user/file.txt
```


**2、建立可访问用户和组**

建立系统账号（只能用于共享）
```
#useradd alice -s /bin/nologin
#useradd jack -s /bin/nologin
#useradd tom -s /bin/nologin
#useradd RD -s/ bin/nologin
```

修改用户组（增加附加组）
```
#usermod-aG RD alice
#usermod-aG RD jack
#usermod-aG RD tom
```

如下添加一个，并检査:

![](https://github.com/Lumnca/Linux/blob/master/Img/a31.png)

添加用户(tom)访问密码：

`smbpasswd -a tom`

然后输入两次即可：

```
New SMB password:
Retype new SMB password:
```

**3、修改目录权限**

```
#chgrp RD /smb/all
#chgrp RD /smb/user
#chown RD /smb/all
#chown RD /smb/user
#chmod 775 /smb/all  ->user group 完全控制
#chmod 775 /smb/user
```


**4、配置smb.conf**

备份原文件

```
#cd /etc/samba
#cp smb.conf smb.conf.bak
```
删除原文件内容`echo '' > smb.conf` 添加如下配置：

```

[global]
        workgroup = WORKGROUP
        server string = My Samba Server
        netbios name = MySamba
        security = user          
        map to guest = Bad User  //共享模式
        passdb backend = tdbsam

[SHAREDOCS]   ----------> windows显示的文件夹名称
        path = /smb/all
        public = yes
        readonly = yes
        browseable = yes

[RDDOCS]     ----------> windows显示的文件夹名称
        path = /smb/user
        valid users = @RD
        write list = @RD
        public = no
        create mask = 0644
        directory mask = 0755
```

关闭防火墙和SeaLinux，在任意地址栏输入 ： \\你的地址即可。

如`\\XXX.XXX.XXX.XXX`

如果需要输入密码凭证输入前面你设置的用户密码即可：

![](https://github.com/Lumnca/Linux/blob/master/Img/a32.png)

出现文件夹表示配置成功！


***

<b id='a3'></b>

### :game_die:NTFS配置 ###

:arrow_double_up:[返回目录](#t)

**:one:概念**

* NFS（Network File System/网络文件系统）
* NFS是FreeBSD支持的文件系统中的一种，由Sun公司开发，于1984年向外公布
* NFS允许网络中的计算机之间通过TCP/IP网络共享资源
* 在NFS的应用中，本地NFS的客户端应用可以透明地读写位于远端NFS服务器上的文件，就像访问本地文件一样
* NFS类似于Windows中的映射网络驱动器功能

**:two:特点**

```
1、NFS用于设置Linux系统之间的文件共享；
2、NFS只是一种文件系统，本身没有传输功能，是基于RPC协议实现的，才能达到两个Linux系统之间的文件目录共享；
3、NFS为C/S架构；
```

**:three:优点**

```
1、节省本地存储空间，将常用的数据存放在一台NFS服务器上且可以通过网络访问，那么本地终端将可以减少自身存储空间的使用。
2、用户不需要在网络中的每个机器上都建有Home目录，Home目录可以放在NFS服务器上且可以在网络上被访问使用。
3、一些存储设备可以在网络上被别的机器使用，提高设备利用率
```

**:four:安装nfs（自动关联rpc）**

>#yum install -y nfs-utils

运行：

设置开机启动：

>#systemctl enable rpcbind

>#systemctl enable nfs-server

运行服务：

>#systemctl start rpcbind

>#systemctl start nfs-server

**:five:相关命令**

`rpcinfo` 显示那些使用portmap注册的程序的信息，并向程序进行RPC调用，检查它们是否正常运行
`exportfs` nfs服务命令，用于查看状态，加载配置文件等,如：exportfs-rv重新加载配置文件更多：man exports
`showmount -e [nfs服务器IP]` 检查NFS服务器端是否有目录共享
`nfsstat` 查看NFS的运行状态

配置文件：/etc/export

export文件默认为空，根据需要配置共享文件夹： `/etc/exports`文件内容格式：

`<输出目录>[客户端1选项（访问权限，用户映射，其他）][客户端2选项（访问权限，用户映射，其他）]…`

`输出目录`

输出目录是指NFS系统中需要共享给客户机使用的目录；

`客户端`

客户端是指网络中可以访问这个NFS输出目录的计算机客户端常用的指定方式：

指定ip地址的主机：192.168.240.132

指定子网中的所有主机：192.168.0.0/24 192.168.0.0/255.255.255.0

指定域名的主机：david.bsmart.cn

指定域中的所有主机：*.bsmart.cn

所有主机：*

`选项`

选项用来设置输出目录的访问权限、用户映射等NFS主要有3类选项：访问权限选项

* 设置输出目录只读：ro
* 设置输出目录读写：rw

`用户映射选项`

* all_squash：将远程访问的所有普通用户及所属组都映射为匿名用户或用户组（nfsnobody）；
* no_all_squash：与all_squash取反（默认设置）；
* root_squash：将root用户及所属组都映射为匿名用户或用户组（默认设置）；
* no_root_squash：与rootsquash取反；

anonuid=xxx：将远程访问的所有用户都映射为匿名用户，并指定该用户为本地用户（UID=xxx）默认为nfsnobody（65534）；anongid=xxx：将远程访问的所有用户组都映射为匿名用户组账户，并指定该匿名用户组账户为本地用户组账户（GID=xxx），默认为nfsnobody（65534）；

`其它选项`

* secure：限制客户端只能从小于1024的tcp/ip端口连接nfs服务器（默认设置）；insecure：允许客户端从大于1024的tcp/ip端口连接服务器；* sync：将数据同步写入内存缓冲区与磁盘中，效率低，但可以保证数据的一致性；async：将数据先保存在内存缓冲区中，必要时才写入磁盘；

**:six:配置步骤**

第一步主机建立共享文件夹/temp

>mkdir /temp

第二步配置文件:vim /etc/exports,并添加如下内容：

`/temp  *(rw,fsid=0,anonuid=65534,anongid=65534)`  

/temp 为共享文件夹 修改权限：`chmod 777 /temp`

第三部：启动服务关闭防火墙和SeaLinux。

第四部访问：

```
showmount -e 47.106.254.86      ---查看该主机是否有共享文件夹
mount -t nfs4 47.106.254.86:/ /mnt/nfs        ---挂载在/mnt/nfs   文件夹下
```

操作如下图：

![](https://github.com/Lumnca/Linux/blob/master/Img/a33.png)



