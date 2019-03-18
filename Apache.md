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

设置开机自启：

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




