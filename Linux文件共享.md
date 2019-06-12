<b id='t'></b>

# :jack_o_lantern:Linux文件共享 #

***

:arrow_double_down:[Samba](#a1)


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
·系统用户密码和Samba用户的密码是不同的
·命令：#smbpasswd
·作用：设置Samba用户密码
·其原理是通过读取/etc/passwd文件中存在的用户名，并设置密码
·示例：
·#smbpasswd alice
```
