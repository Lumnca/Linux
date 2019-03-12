# :tennis:Shell

<p id="t"></p>

:arrow_down_small:[Shell简介](https://github.com/Lumnca/Linux/edit/master/shell.md#a1)

:arrow_down_small:[创建Shell脚本](https://github.com/Lumnca/Linux/edit/master/shell.md#a2)

<p id="a1"></p>

### :rugby_football:Shell简介  ###

:arrow_double_up:[返回目录](https://github.com/Lumnca/Linux/edit/master/shell.md#p)

* Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

* Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

* Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

**shell脚本**

>Shell 脚本（shell script），是一种为 shell 编写的脚本程序。业界所说的 shell 通常都是指 shell 脚本，但读者朋友要知道，shell 和 shell script 是两个不同的概念。由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。
Shell 脚本（shell script），是一种为 shell 编写的脚本程序。业界所说的 shell 通常都是指 shell 脚本，但读者朋友要知道，shell 和 shell script 是两个不同的概念。由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。

Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，常见的有：

  * Bourne Shell（/usr/bin/sh或/bin/sh）
  * Bourne Again Shell（/bin/bash）
  * C Shell（/usr/bin/csh）
  * K Shell（/usr/bin/ksh）
  * Shell for Root（/sbin/sh）
……

本教程关注的是 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 #!/bin/sh，它同样也可以改为 #!/bin/bash。

**#! 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。**

<p id="a2"></p>

### :rugby_football:创建Shell脚本  ###

:arrow_double_up:[返回目录](https://github.com/Lumnca/Linux/edit/master/shell.md#p)

首先创建一个名为shell脚本文件(.sh为shell文件名)：

```linux
touch shell.sh
```

然后我们再打开这个文件并编辑(vim shell.sh)：

```shell
#!/bin/bash
echo "Hello World !"
```

第一行要为 `#!/bin/bash`说明这个一个shell文件，echo 命令用于向窗口输出文本。

创建编译：











