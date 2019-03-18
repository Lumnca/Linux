# :globe_with_meridians: Apache服务器



***

### :earth_americas:Apache简介 ###

>Apache是世界使用排名第一的Web服务器软件。它可以运行在几乎所有广泛使用的计算机平台上，由于其跨平台和安全性被广泛使用，是最流行的Web服务器端软件之一。它快速、可靠并且可通过简单的API扩充，将Perl/Python等解释器编译到服务器中。同时Apache音译为阿帕奇，是北美印第安人的一个部落，叫阿帕奇族，在美国的西南部。也是一个基金会的名称、一种武装直升机等等。

目前能够提供Web网络服务的程序有IIS，Nginx，和Apache等，其中IIS是Windows系统中默认的Web服务程序。但是IIS只能在Windows系统中使用，Nginx程序作为一款轻量级的的网站服务软件，因其稳定性和丰富的功能而快速占领服务器市场，Nginx最被认可的还当是系统资源消耗极低且并发能力极强。Apache程序是目前拥有很高市场占有率的Web服务程序之一，其跨平台和安全性广泛被认可且拥有快速，可靠，简单的API扩展。

***

### :earth_americas:Apache安装 ###

使用命令行安装程序：

```shell
yum install httpd
```

出现安装信息，最后会出现如下提示，输入y继续：

![](https://github.com/Lumnca/Linux/blob/master/Img/a1.png)

安装结束后自动退出停止。

设置开机自启以及启动http服务：

```shell
systemctl start httpd
systemctl enable httpd
```

### :earth_americas:配置服务文件参数 ###

在安装后，如果你需要配置一些参数才能是你的服务器工作。最主要的就是配置文件。配置文件所在的文件夹如下表：

|配置文件的名称|所在文件|
|:--:|:--:|
|服务目录|/etc/httpd|
|主配置文件|/etc/httpd/conf/httpd.conf|
|网站数据目录|/var/www/html|
|访问日志|/var/log/httpd/access_log|
|错误日志|/var/log/httpd/error_log|


首先修改主配置文件：

```shell
 vim /etc/httpd/conf/httpd.conf
```

你会发现这个配置文件有很多内容，达到了353行，但是很多行前面都带了#，说明这只是说明文段。切记不要随意删除或者修改配置文件的内容，一定要按照要求修改。

所以你需要了解没有#的段落就行，下面给出具体含义：

|参数|说明|
|:--:|:--:|
|ServerRoot|服务器根目录|
|ServerAdmin|管理员邮箱|
|User|运行服务的用户|
|Group|运行服务的用户组|
|ServerName|网站服务器的域名|
|DocumentRoot|网页数据根目录|
|Directory|网站数据目录的权限|
|Listen|监听的IP地址与端口号|
|DirectoryIndex|默认的索引页页面|
|ErrorLog|错误日志文件|
|CustomLog|访问日志文件|
|Timeout|网页超时时间，默认300秒|

现在还是不要对文件进行修改，均采用默认设置。为了使其他机器能够访问到我们服务器的网站，我们还需要开启http服务与防火墙。如果你的服务器或者虚拟机已经把80（默认端口，可以修改）端口设置为可以访问，那么你就可以直接输入自己的IP：80就可以看到Apache的默认网页。如果不能访问就需要设置打开80端口。

***

### :earth_americas:SELinux安全子系统 ###

SELinux是美国国家安全局在Linux开源社区的帮助下开发的一个强制访问控制的安全子系统。配置这个对黑客入侵服务器，也无法利用系统内部的服务程序进行越权操作。由于这项技术比较难，导致很多人都没有在部署Linux系统后直接讲SELinus禁用了，这绝对不是明智的选择。

SELinux有三种模式：

 * enforcing ： 强制启用安全策略模式，将拦截服务的不合法请求
 
 * permissive ： 遇到服务越权访问时，只发出警告而不强制拦截。
 
 * disabled ： 对于越权的行为不警告也不拦截。
 
 禁用SELinux服务后可以减少报错几率，但是不是太建议这样做，我们可以查看自己系统的默认模式：
 
 ```
  vim /etc/selinux/config
 ```
 
 ![](https://github.com/Lumnca/Linux/blob/master/Img/a2.png)
 
可以看到我们的默认形式是disabled，我们可以把它修改为enforcing，如下在SELINUX=修改为

`SELINUX=enforcing`

修改后并不会马上生效，需要重启才能看到运行模式，使用getenforce查看当前模式：

```
getenforce
```

重启再使用getenforce就可以更改模式了，使用setenforce 0|1 就可以启用当前或者禁用当前模式

```
setenforce 0   //禁用
setenforce 1   //启用
```

但是这个仅限于当前，重启之后就会恢复原样。

**SELinux安全上下文**

SELinux安全上下文是由用户段，角色段以及类型段等多个信息项共同组成的，其中，用户段system_u代表系统用户进程的身份，角色段object_r代表文件目录的角色，类型段httpd_sys_content_t代表网站服务的系统文件。semanage命令可以设置文件，目录。常用参数如下：

 * -l参数用于查询
 * -a参数用于添加
 * -m参数用于修改
 * -d参数用于删除
 
 如果，你修改了网站的数据目录，就可以在新的目录中添加一条SELinux上下文，让这个目录可以被httpd服务程序访问：
 
 ```
 semanage fcontext -a -t httpd_sys_content_t /home/www/abc(新目录)
 ```

设置好后使用

```
restorecon  -Rv /home/www/abc
```

当然这SELinux安全上下文部分可以不用设置。只是介绍有相关操作。
