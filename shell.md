# :tennis:Shell

<p id="t"></p>

:arrow_down_small:[Shell简介](#a1)

:arrow_down_small:[创建Shell脚本](#a2)

:arrow_down_small:[基本语法](#a3)

<p id="a1"></p>

### :rugby_football:Shell简介  ###

:arrow_double_up:[返回目录](#t)

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

:arrow_double_up:[返回目录](#t)

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

* `方法1：[在子shell中执行]文件作为系统shell的参数`

>#bash first.sh

* `方法2：[在子shell中执行]将脚本文件权限设为可执行，再执行`

>#chmod u+x first.sh

>#./ first.sh

* `方法3：在当前shell中执行，前面加source或.[空格]，再执行`

>#source first.sh或#.first.sh

***

<p id="a3"></p>

### :rugby_football:基本语法  ###

:arrow_double_up:[返回目录](#t)

**:one:变量**

shell中变量默认没有类型（弱类型），无类型声明

定义变量：

my_var="var_value"

注意：变量名和等号之间（等号两边）**不能有空格**

使用变量：

* 1、变量前加$
* 2、加：可选，帮助解释器识别变量的边界

```shell
echo $my_var 
echo"I am good at ${my_var）Script"
```

修改变量

直接重新赋值:`my_var="another_var_value"`

从键盘输入变量值: `read [-p输出] 变量名`

```shell
read varl var2…
read-p "input:" var1
```

注释,“#”开头,没有多行注释，只能每一行加#.

如下输入一个名字，并问好：

![](https://github.com/Lumnca/Linux/blob/master/Img/a28.png)

然后编译执行:

![](https://github.com/Lumnca/Linux/blob/master/Img/a29.png)

系统变量

```
$0        shell程序的执行名字

$n        shell程序外部的第n个参数值，n=1..9

$*        shell程序的所有参数，此选项数可超过9个

$#        shell程序的参数个数

$$        shell程序的PID（脚本运行的当前进程ID号）

$！       执行上一个背景指令的PID（后台运行的最后一个进程的进程ID号）

$？       执行上一个指令的返回值（显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误）

$-        显示shell使用的当前选项，与set命令功能相同

$@        跟$*类似，但是可以当作数组用
```

示例：

```shell
#!/bin/bash

echo '脚本名称:' $0
echo '参数个数:' $#
echo '第一个参数:' $1
echo '第二个参数:' $2
echo '所有参数内容:' $*
```

运行脚本，参数传递只需要在执行命令时加空格即可。

![](https://github.com/Lumnca/Linux/blob/master/Img/a30.png)

**:two:shell执行命令行**

在shell执行命令直接换行写入即可，只不过需要一行一个命令：

```
#!/bin/bash

mkdir aaa
touch abc.txt
ls
```

命令转化为变量的两种方式：

```
$(命令行)
`命令行`
```

如：

```shell
#!/bin/bash
var=$(ls)
echo "当前文件夹内容:" $var
```

**:three:数字运算**

语法：`expr integer operator integer`
operator 包括：`+-*/%`

注意：
`对*的使用要用转义符\`
`如：expr 5\*10        #乘法运算：5*10`

Bash shell的算术运算有四种方式:

1、使用expr外部程式,如：`r=expr 4+5`

2、使用$（（））如：`r=$（（4+5））`#注意内部数字两边要有空格

3、使用$[]-推荐 如：`r=$[4+5]` 

4、使用let命令

```
1et "n=m*5"
1et "n--"
```

如：

```shell
var=$[5+1*2-3]

echo "计算结果:" $var
```

**:four:字符串**

```
字符串是shell编程中最常用、最有用的数据类型
字符串可以用单引号，也可以用双引号，也可以不用引号
单双引号的区别跟PHP类似
单引号：
str='this is a string'
单引号里的任何字符都会原样输出，字符串中的变量无效
单引号字串中不能出现单引号(双引号中可以出现单引号)
```

字符串:

* 双引号：
* 双引号里可以有变量
* 双引号里可以出现转义字符

```
my_name='amazon'
str="Hello，I know your are\"$my_name\"！\n"
```

获取字符串长度：

```
string="abcd"
echo ${#string}
```
#输出：4

提取子字符串:

```
string="alibaba is a great company"
echo ${string：1：4}
#输出：liba
```

**:five:数组**

数组定义：`数组名[index]=value`

```shell
NAME[0]="Zara"
NAME[1]="Qadir"
NAME[2]="Mahnaz"
NAME[3]="Ayan"
输出数组中的元素
echo"First Index：$HNAME[O]}"
echo"Second Index：${NAME[1]}"
```

**:six:测试**

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

`数值测试`：

|参数|	说明|
|:-:|:----:|
|-eq|	等于则为真|
|-ne	|不等于则为真|
|-gt	|大于则为真|
|-ge|	大于等于则为真|
|-lt|	小于则为真|
|-le|	小于等于则为真|

示例：

```shell
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

`字符串测试 `:

|参数|	说明|
|:-:|:----:|
|=	|等于则为真|
|!=|	不相等则为真|
|-z 字符串|	字符串的长度为零则为真|
|-n 字符串|字符串的长度不为零则为真|

示例：

```shell
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
```

`文件测试`

|参数|	说明|
|:--:|:---:|
|-e 文件名|	如果文件存在则为真|
|-r 文件名|	如果文件存在且可读则为真|
|-w 文件名	|如果文件存在且可写则为真|
|-x 文件名|	如果文件存在且可执行则为真|
|-s 文件名|	如果文件存在且至少有一个字符则为真|
|-d 文件名|	如果文件存在且为目录则为真|
|-f 文件名|	如果文件存在且为普通文件则为真|
|-c 文件名	|如果文件存在且为字符型特殊文件则为真|
|-b 文件名|	如果文件存在且为块特殊文件则为真|

示例：

```
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```

And和Or

```
[[ command1 && command2 ]]
[[ command1 || command2 ]]
```

**:seven:流程控制**

if
sh的流程控制不可为空
如果else分支没有语句执行，不能写空的else

**1.is-else语句格式：**

```shell
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```

**2.if else-if else 语法格式:**

```shell
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

如：

```shell
if [ $1 -gt 0 ]          ----注意变量左右均有空格
then
        echo "是一个正数"
elif [ $1 -eq 0 ]
then
        echo "是一个0"
else
        echo "负数"
fi
```

**3.for循环**

格式：

```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

当变量值在列表里，for循环即执行一次所有命令，使用变量名获取列表中的当前取值。命令可为任何有效的shell命令和语句。in列表可以包含替换、字符串和文件名。

in列表是可选的，如果不用它，for循环使用命令行的位置参数。

例如，顺序输出当前列表中的数字：

```
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

输出结果：

```
The value is: 1
The value is: 2
The value is: 3
The value is: 4
The value is: 5
```

**4.while 语句**

while循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件。其格式为：

```
while condition
do
    command
done
```

以下是一个基本的while循环，测试条件是：如果int小于等于5，那么条件返回真。int从0开始，每次循环处理时，int加1。运行上述脚本，返回数字1到5，然后终止。

```
#!/bin/bash

i=0

while (($i < 4 ))
do
        echo $i
        i=$[$i+1]
done

```

**5.case语句**

Shell case语句为多选择语句。可以用case语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。case语句格式如下：


```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

case工作方式如上所示。取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

下面的脚本提示输入1到4，与每一种模式进行匹配：

```
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

**6.函数**

格式与JavaScript函数格式一样：

```
function fun()
{
     命令
     
     返回值（可无）
}
```

* 1、函数声明关键字function可选。
* 2、函数返回值，可以显示加return返回，如果不加，将以最后一条命令运行结果作为返回值。
* return只能返回数值n（O-255）
* 3、必须在调用函数地方之前声明函数，shell脚本是逐行运行。
* 4、调用：returnvalue=$（funname 32）；在shell中单括号里面是命令语句可以将shell中函数看作是定义一个新的命令各个输入参数直接用空格分隔，函数里面获得参数通过：$1…$n得到
* 5、函数返回值只能通过$？系统变量获得，直接通过=获得是空值。函数是一个命令，在shell获得命令返回值，都需要通过$？获得。
* 6、如果需要传出其它类型函数值，可以在函数调用之前，定义变量（全局变量）在函数内部可以直接修改，然后在执行函数可以读出修改值。
* 7、定义自己的变量（局部变量）：local变量=值，（内部变量）内部变量的修改不会影响函数外部同名变量的值


无参数无返回值函数：

```
#!/bin/bash

function a()
{
        echo "hello world!"
}

a   ------调用
```

有参数函数：

```
#!/bin/bash

function a()
{
        echo "hello world!" $1
}

a  lumnca   ------调用加传值
```

有参数有返回值函数：

```
#!/bin/bash

function a()
{
        echo $[$1+$2]
        return $[$1+$2]
}


a 4 5   -------执行函数

echo $?      --------获取返回数据
```



