

# linux基础

## linux命令行基础

#### 常用命令

```shell
(1) ctrl c: 取消命令，并且换行
(2) ctrl u: 清空本行命令
(3) tab键：可以补全命令和文件名，如果补全不了快速按两下tab键，可以显示备选选项
(4) ls: 列出当前目录下所有文件，蓝色的是文件夹，白色的是普通文件，绿色的是可执行文件
(5) pwd: 显示当前路径
(6) cd XXX: 进入XXX目录下, cd .. 返回上层目录
(7) cp XXX YYY: 将XXX文件复制成YYY，XXX和YYY可以是一个路径，比如../dir_c/a.txt，表示上层目录下的dir_c文件夹下的文件a.txt
(8) mkdir XXX: 创建目录XXX
(9) rm XXX: 删除普通文件;  rm XXX -r: 删除文件夹 -f强制
(10) mv XXX YYY: 将XXX文件移动到YYY，和cp命令一样，XXX和YYY可以是一个路径；重命名也是用这个命令
(11) touch XXX: 创建一个文件
(12) cat XXX: 展示文件XXX中的内容
(13) find 查找文件 find /path -name '*keyword*' | grep  '\.so$'  *表示通配符 $表示明确以.so结尾
(14) ag 查找代码中关键词 sudo ag libopenjp2 /usr
(15) ps -ef |grep python
(16) kill -9 id #杀死进程 传递某个具体的信号：kill -s SIGTERM pid
(17) cat /proc/cpuinfo #查看cpu信息
(18) top #查看所有进程信息 M按内存排序 P按照使用CPU排序
(19) df -h #查看磁盘使用
(20) free -h #查看内存使用
(21) du -sh #查看当前目录占用的硬盘空间
(22) tar -zxvf xxx.tar.gz -C yyy #指定解压目录
(23) tar -zcvf /home/jym/temp.tar.gz ./* 
(24) dpkg -l |grep boost
(25) dpkg -L libboost-dev | grep '\.so'  
#！！！ 注意\这种方法与13不同的是未启用正则表达式 .so.1会被找到但是13只会找.so结尾文件
(26) ip addr show#显示网卡
(27) ip route show#查看路由表
(28) ip route del default via 192.168.43.1 dev usb0#删除路由表
(29) nmcli device shown
(30) nmcli　#查看ip信息
(31) nmcli device status　  ##所有接口的简略信息
(32) nmcli device show ens160    ##指定的ens160网络接口的详细信息



drwxrwxrwx #是否文件夹/自己/同组/其他人
chmod：修改文件权限
chmod 777 xxx -R：递归修改整个文件夹的权限
```

![image-20240630155725005](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630155725005.png)

![image-20240630155638575](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630155638575.png)

![image-20240630165415429](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630165415429.png)

![image-20240630165434728](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630165434728.png)

#### tmux 

```shell
1，输入命令tmux使用工具

2，上下分屏：ctrl + b  再按 "

3，左右分屏：ctrl + b  再按 %

4，切换屏幕：ctrl + b  再按o

5，关闭一个终端：ctrl + b  再按x

6，上下分屏与左右分屏切换： ctrl + b  再按空格键

以下记得把ctrl+a换成ctrl+b
功能：
    (1) 分屏。
    (2) 允许断开Terminal连接后，继续运行进程。
结构：
    一个tmux可以包含多个session，一个session可以包含多个window，一个window可以包含多个pane。
    实例：
        tmux:
            session 0:
                window 0:
                    pane 0
                    pane 1
                    pane 2
                    ...
                window 1
                window 2
                ...
            session 1
            session 2
            ...
操作：
    (1) tmux：新建一个session，其中包含一个window，window中包含一个pane，pane里打开了一个shell对话框。
    (2) 按下Ctrl + a后手指松开，然后按%：将当前pane左右平分成两个pane。
    (3) 按下Ctrl + a后手指松开，然后按"（注意是双引号"）：将当前pane上下平分成两个pane。
    (4) Ctrl + d：关闭当前pane；如果当前window的所有pane均已关闭，则自动关闭window；如果当前session的所有window均已关闭，则自动关闭session。
    (5) 鼠标点击可以选pane。
    (6) 按下ctrl + a后手指松开，然后按方向键：选择相邻的pane。
    (7) 鼠标拖动pane之间的分割线，可以调整分割线的位置。
    (8) 按住ctrl + a的同时按方向键，可以调整pane之间分割线的位置。
    (9) 按下ctrl + a后手指松开，然后按z：将当前pane全屏/取消全屏。
    (10) 按下ctrl + a后手指松开，然后按d：挂起当前session。
    (11) tmux a：打开之前挂起的session。
    (12) 按下ctrl + a后手指松开，然后按s：选择其它session。
        方向键 —— 上：选择上一项 session/window/pane
        方向键 —— 下：选择下一项 session/window/pane
        方向键 —— 右：展开当前项 session/window
        方向键 —— 左：闭合当前项 session/window
    (13) 按下Ctrl + a后手指松开，然后按c：在当前session中创建一个新的window。
    (14) 按下Ctrl + a后手指松开，然后按w：选择其他window，操作方法与(12)完全相同。
    (15) 按下Ctrl + a后手指松开，然后按PageUp：翻阅当前pane内的内容。
    (16) 鼠标滚轮：翻阅当前pane内的内容。
    (17) 在tmux中选中文本时，需要按住shift键。（仅支持Windows和Linux，不支持Mac，不过该操作并不是必须的，因此影响不大）
    (18) tmux中复制/粘贴文本的通用方式：
        (1) 按下Ctrl + a后松开手指，然后按[
        (2) 用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
        (3) 按下Ctrl + a后松开手指，然后按]，会将剪贴板中的内容粘贴到光标处

在家目录下创建.tmux.conf，并粘贴下面内容保存后，进入tmux， ctrl+b，然后输入命令：source-file ~/.tmux.conf 即可(或 在bash下执行tmux source ~/.tmux.conf)
```

#### vim

```
功能：
    (1) 命令行模式下的文本编辑器。
    (2) 根据文件扩展名自动判别编程语言。支持代码缩进、代码高亮等功能。
    (3) 使用方式：vim filename
        如果已有该文件，则打开它。
        如果没有该文件，则打开个一个新的文件，并命名为filename
模式：
    (1) 一般命令模式
        默认模式。命令输入方式：类似于打游戏放技能，按不同字符，即可进行不同操作。可以复制、粘贴、删除文本等。
    (2) 编辑模式
        在一般命令模式里按下i，会进入编辑模式。
        按下ESC会退出编辑模式，返回到一般命令模式。
    (3) 命令行模式
        在一般命令模式里按下:/?三个字母中的任意一个，会进入命令行模式。命令行在最下面。
        可以查找、替换、保存、退出、配置编辑器等。
操作：
    (1) i：进入编辑模式
    (2) ESC：进入一般命令模式
    (3) h 或 左箭头键：光标向左移动一个字符
    (4) j 或 向下箭头：光标向下移动一个字符
    (5) k 或 向上箭头：光标向上移动一个字符
    (6) l 或 向右箭头：光标向右移动一个字符
    (7) n<Space>：n表示数字，按下数字后再按空格，光标会向右移动这一行的n个字符
    (8) 0 或 功能键[Home]：光标移动到本行开头
    (9) $ 或 功能键[End]：光标移动到本行末尾
    (10) G：光标移动到最后一行
    (11) :n 或 nG：n为数字，光标移动到第n行
    (12) gg：光标移动到第一行，相当于1G
    (13) n<Enter>：n为数字，光标向下移动n行
    (14) /word：向光标之下寻找第一个值为word的字符串。
    (15) ?word：向光标之上寻找第一个值为word的字符串。
    (16) n：重复前一个查找操作
    (17) N：反向重复前一个查找操作
    (18) :n1,n2s/word1/word2/g：n1与n2为数字，在第n1行与n2行之间寻找word1这个字符串，并将该字符串替换为word2
    (19) :1,$s/word1/word2/g：将全文的word1替换为word2
    (20) :1,$s/word1/word2/gc：将全文的word1替换为word2，且在替换前要求用户确认。
    (21) v：选中文本
    (22) d：删除选中的文本
    (23) dd: 删除当前行
    (24) y：复制选中的文本
    (25) yy: 复制当前行
    (26) p: 将复制的数据在光标的下一行/下一个位置粘贴
    (27) u：撤销
    (28) Ctrl + r：取消撤销
    (29) 大于号 >：将选中的文本整体向右缩进一次
    (30) 小于号 <：将选中的文本整体向左缩进一次
    (31) :w 保存
    (32) :w! 强制保存
    (33) :q 退出
    (34) :q! 强制退出
    (35) :wq 保存并退出
    (36) :set paste 设置成粘贴模式，取消代码自动缩进
    (37) :set nopaste 取消粘贴模式，开启代码自动缩进
    (38) :set nu 显示行号
    (39) :set nonu 隐藏行号
    (40) gg=G：将全文代码格式化
    (41) :noh 关闭查找关键词高亮
    (42) Ctrl + q：当vim卡死时，可以取消当前正在执行的命令
    
异常处理：
    每次用vim编辑文件时，会自动创建一个.filename.swp的临时文件。
    如果打开某个文件时，该文件的swp文件已存在，则会报错。此时解决办法有两种：
        (1) 找到正在打开该文件的程序，并退出
        (2) 直接删掉该swp文件即可
退到后台 fg返回
vim中复制需要先按住shift
每次复制少一个字符是因为没有切换到编辑模式
```

#### shell语法

##### 注释

````sh
#### 

```sh
#！ /bin/bash 
#第一行写如上：第一行的内容指定了shell脚本解释器的路径，而且这个指定路径只能放在文件的第一行。第一行写错或者不写时，系统会有一个默认的解释器进行解释

多行注释
格式：
:<<EOF
left
middle
right
EOF
其中EOF可以换成其它任意字符串。例如abc等等
````

##### 变量

使用变量
**使用变量，需要加上$符号，或者${}符号。**花括号是可选的，主要为了帮助解释器识别变量边界。

```sh
name=yxc
echo $name  # 输出yxc
echo ${name}  # 输出yxc
echo ${name}acwing  # 输出yxcacwing
```

**只读变量**
使用readonly或者declare可以将变量变为只读。

```sh
name=yxc
readonly name
declare -r name  # 两种写法均可

name=abc  # 会报错，因为此时name只读
```

**删除变量**
unset可以删除变量。

```sh
name=yxc
unset name
echo $name  # 输出空行
```

**变量类型**
1.自定义变量（局部变量）
	子进程不能访问的变量
2.环境变量（全局变量）
	子进程可以访问的变量
自定义变量改成环境变量：

```
acs@9e0ebfcd82d7:~$ name=yxc  # 定义变量
acs@9e0ebfcd82d7:~$ export name  # 第一种方法
acs@9e0ebfcd82d7:~$ declare -x name  # 第二种方法
```

环境变量改为自定义变量：

```
acs@9e0ebfcd82d7:~$ export name=yxc  # 定义环境变量
acs@9e0ebfcd82d7:~$ declare +x name  # 改为自定义变量
```

**字符串**
字符串可以用单引号，也可以用双引号，也可以不用引号。

单引号与双引号的区别：

- 单引号中的内容会原样输出，不会执行、不会取变量；

- 双引号中的内容可以执行、可以取变量；

```
name=yxc  # 不用引号
echo 'hello, $name \"hh\"'  # 单引号字符串，输出 hello, $name \"hh\"
echo "hello, $name \"hh\""  # 双引号字符串，输出 hello, yxc "hh"
```

获取字符串长度

```
name="yxc"
echo ${#name}  # 输出3
```

提取子串

```
name="hello, yxc"
echo ${name:0:5}  # 提取从0开始的5个字符
```

${}取大括号的值 $()取stdout的值

在Shell脚本中，`${}` 是一种参数扩展（parameter expansion）的语法，用于对变量的值进行操作或格式化。以下是一些常见的使用场景：

1. **变量替换**：获取变量的值。

   ```
   bashvar="hello"
   echo ${var}  # 输出：hello
   ```

2. **默认值**：如果变量未设置或为空，可以使用默认值。

   ```
   bash
   echo ${var:-default_value}  # 如果 var 未设置或为空，输出 default_value
   ```

3. **去除变量值的前导/尾随字符**：

   - 去除前导空白符：

     ```
     bashvar="  hello  "
     echo ${var#"  "}  # 输出：hello
     ```

   - 去除尾随空白符：

     ```
     bash
     echo ${var%"  "}  # 输出：  hello
     ```

4. **截取子字符串**：从变量中截取特定部分。

   ```
   bashvar="hello world"
   echo ${var:6}  # 从第7个字符开始截取，输出：world
   echo ${var:0:5}  # 从第1个字符开始截取5个字符，输出：hello
   ```

5. **长度**：获取变量值的长度。

   ```
   bashvar="hello"
   echo ${#var}  # 输出：5
   ```

6. **模式匹配**：对变量值进行模式匹配。

   - 匹配模式的开始部分：

     ```
     bashvar="hello world"
     echo ${var#"h*l"}  # 输出：o world
     ```

   - 匹配模式的结束部分：

     ```
     bash
     echo ${var%"*d"}  # 输出：hello wo
     ```

7. **数组索引**：访问数组元素。

   ```
   basharray=("apple" "banana" "cherry")
   echo ${array[1]}  # 输出：banana
   ```

8. **数组长度**：获取数组的长度。

   ```
   bashecho ${#array[@]}  # 输出数组元素的个数
   echo ${#array[1]}  # 输出数组第一个元素的长度
   ```

9. **环境变量**：访问环境变量。

   ```
   bash
   echo ${PATH}
   ```

10. **命令替换**：执行命令并将输出替换到当前位置。

    ```
    bash复制
    echo ${date}  # 执行 date 命令并输出结果
    ```

这些是 `${}` 在Shell脚本中的一些基本用法。通过不同的语法和选项，可以对变量进行各种复杂的操作。

##### 默认变量

**文件参数变量**
在执行shell脚本时，可以向脚本传递参数。$1是第一个参数，$2是第二个参数，以此类推。特殊的，$0是文件名（包含路径）。例如：

创建文件test.sh：

```sh
#! /bin/bash

echo "文件名："$0
echo "第一个参数："$1
echo "第二个参数："$2
echo "第三个参数："$3
echo "第四个参数："$4
```

然后执行该脚本：

```sh
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh 1 2 3 4
文件名：./test.sh
第一个参数：1
第二个参数：2
第三个参数：3
第四个参数：4
```

**其它参数相关变量**

| 参数       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| $#         | 代表文件传入的参数个数，如上例中值为4                        |
| $*         | 由所有参数构成的用空格隔开的字符串，如上例中值为"$1 $2 $3 $4" |
| $@         | 每个参数分别用双引号括起来的字符串，如上例中值为"$1" "$2" "$3" "$4" |
| $$         | 脚本当前运行的进程ID                                         |
| $?         | 上一条命令的退出状态（注意不是stdout，而是exit code）。0表示正常退出，其他值表示错误 |
| $(command) | 返回command这条命令的stdout（可嵌套）                        |
| \`commond` | 返回command这条命令的stdout（不可嵌套）                      |

##### 数组

数组中可以存放多个不同类型的值，只支持一维数组，初始化时不需要指明数组大小。
数组下标从0开始。 

**定义**
数组用小括号表示，元素之间用空格隔开。例如：

```sh
array=(1 abc "def" yxc)
```

也可以直接定义数组中某个元素的值：

```sh
array[0]=1
array[1]=abc
array[2]="def"
array[3]=yxc
```

**读取数组中某个元素的值**
格式：

```sh
${array[index]}
```

例如：

```sh
array=(1 abc "def" yxc)
echo ${array[0]}
echo ${array[1]}
echo ${array[2]}
echo ${array[3]}
```

读取整个数组
格式：

```sh
${array[@]}  # 第一种写法
${array[*]}  # 第二种写法
```

例如：

```sh
array=(1 abc "def" yxc)

echo ${array[@]}  # 第一种写法
echo ${array[*]}  # 第二种写法
```

**数组长度**
类似于字符串

```sh
${#array[@]}  # 第一种写法
${#array[*]}  # 第二种写法
```

例如：

```sh
array=(1 abc "def" yxc)

echo ${#array[@]}  # 第一种写法
echo ${#array[*]}  # 第二种写法
```

##### expr

expr命令用于求表达式的值，格式为：

**expr 表达式**
表达式说明：

用空格隔开每一项
用反斜杠放在shell特定的字符前面（发现表达式运行错误时，可以试试转义）
对包含空格和其他特殊字符的字符串要用引号括起来
**expr会在stdout中输出结果。如果为逻辑关系表达式，则结果为真时，stdout输出1，否则输出0。**
**expr的exit code：如果为逻辑关系表达式，则结果为真时，exit code为0，否则为1。**

**字符串表达式**
`length STRING`
返回STRING的长度
`index STRING CHARSET`
CHARSET中任意单个字符在STRING中最前面的字符位置，下标从1开始。如果在STRING中完全不存在CHARSET中的字符，则返回0。
`substr STRING POSITION LENGTH`
返回STRING字符串中从POSITION开始，长度最大为LENGTH的子串。如果POSITION或LENGTH为负数，0或非数值，则返回空字符串。

示例：

```
str="Hello World!"

echo `expr length "$str"`  # ``不是单引号，表示执行该命令，输出12
echo `expr index "$str" aWd`  # 输出7，下标从1开始
echo `expr substr "$str" 2 3`  # 输出 ell
```

整数表达式
expr支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式。

+ -
加减运算。两端参数会转换为整数，如果转换失败则报错。

* / %
乘，除，取模运算。两端参数会转换为整数，如果转换失败则报错。

() 可以改变优先级，但需要用反斜杠转义

示例：

```
a=3
b=4
+ 
echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)
```

**逻辑关系表达式**

- `|`
  如果第一个参数非空且非0，则返回第一个参数的值，否则返回第二个参数的值，但要求第二个参数的值也是非空或非0，否则返回0。如果第一个参数是非空或非0时，不会计算第二个参数。
- `&`
  如果两个参数都非空且非0，则返回第一个参数，否则返回0。如果第一个参为0或为空，则不会计算第二个参数。
- `< <= = == != >= >`
  比较两端的参数，如果为true，则返回1，否则返回0。”==”是”=”的同义词。”expr”首先尝试将两端参数转换为整数，并做算术比较，如果转换失败，则按字符集排序规则做字符比较。
- `()` 可以改变优先级，但需要用反斜杠转义
  示例：

```sh
a=3
b=4

echo `expr $a \> $b`  # 输出0，>需要转义
echo `expr $a '<' $b`  # 输出1，也可以将特殊字符用引号引起来
echo `expr $a '>=' $b`  # 输出0
echo `expr $a \<\= $b`  # 输出1

c=0
d=5

echo `expr $c \& $d`  # 输出0
echo `expr $a \& $b`  # 输出3
echo `expr $c \| $d`  # 输出5
echo `expr $a \| $b`  # 输出3
```



##### read

read命令用于从标准输入中读取单行数据。当读到文件结束符时，exit code为1，否则为0。

参数说明

- -p: 后面可以接提示信息
- -t：后面跟秒数，定义输入字符的等待时间，超过等待时间后会自动忽略此命令

```
acs@9e0ebfcd82d7:~$ read name  # 读入name的值
acwing yxc  # 标准输入
acs@9e0ebfcd82d7:~$ echo $name  # 输出name的值
acwing yxc  #标准输出
acs@9e0ebfcd82d7:~$ read -p "Please input your name: " -t 30 name  # 读入name的值，等待时间30秒
Please input your name: acwing yxc  # 标准输入
acs@9e0ebfcd82d7:~$ echo $name  # 输出name的值
acwing yxc  # 标准输出
```

##### echo

echo用于输出字符串。命令格式：

echo STRING
**显示普通字符串**
echo "Hello AC Terminal"
echo Hello AC Terminal  # 引号可以省略
**显示转义字符**
echo "\"Hello AC Terminal\""  # 注意只能使用双引号，如果使用单引号，则不转义
echo \"Hello AC Terminal\"  # 也可以省略双引号
**显示变量**

```sh
name=yxc
echo "My name is $name"  # 输出 My name is yxc
显示换行
echo -e "Hi\n"  # -e 开启转义
echo "acwing"
输出结果：
Hi

acwing
```

**显示不换行**

```sh
echo -e "Hi \c" # -e 开启转义 \c 不换行
echo "acwing"

输出结果：
Hi acwing
```

**显示结果定向至文件**

```sh
echo "Hello World" > output.txt  # 将内容以覆盖的方式输出到output.txt中
```

**原样输出字符串，不进行转义或取变量(用单引号)**

```sh
name=acwing
echo '$name\"'
$name\"
```

**显示命令的执行结果**

```sh
echo `date`
Wed Sep 1 11:45:33 CST 2021
```

##### printf

printf命令用于格式化输出，类似于C/C++中的printf函数。

默认**不会在字符串末尾添加换行符**。

命令格式：

```sh
printf format-string [arguments...]
```

**用法示例**
脚本内容：

```sh
printf "%10d.\n" 123  # 占10位，右对齐
printf "%-10.2f.\n" 123.123321  # 占10位，保留2位小数，左对齐
printf "My name is %s\n" "yxc"  # 格式化输出字符串
printf "%d * %d = %d\n"  2 3 `expr 2 \* 3` # 表达式的值作为参数
```

输出结果：

```
       123.
123.12    .
My name is yxc
2 * 3 = 6
```

##### test

 **逻辑运算符&&和||**
-  && 表示与，|| 表示或

- 二者具有短路原则：
  expr1 && expr2：当expr1为假时，直接忽略expr2
  expr1 || expr2：当expr1为真时，直接忽略expr2

- 表达式的exit code为0，表示真；为非零，表示假。（与C/C++中的定义相反）

**test命令**
 在命令行中输入man test，可以查看test命令的用法。

test命令用于判断文件类型，以及对变量做比较。

test命令用exit code返回结果，而不是使用stdout。0表示真，非0表示假。

例如：

```sh
test 2 -lt 3  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ test -e test.sh && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ test -e test2.sh && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

**文件类型判断**
命令格式：

```sh
test -e filename  # 判断文件是否存在
```

| 测试参数 | 代表意义   |
| -------- | ---------- |
| -e       | 是否存在   |
| -f       | 是否为文件 |
| -d       | 是否为目录 |

**文件权限判断**
命令格式：

```sh
test -r filename  # 判断文件是否可读
```

| 测试参数 | 代表意义 |
| -------- | -------- |
| -r        |  可读     |
| -w        |  可写     |
| -x        |  可执行   |
| -s        |  非空     |
| -z | 检查 `STRING` 是否为空（即长度为0）。如果 `STRING` 为空，则返回真（true） |
| -n | 检查 `STRING` 是否非空（即长度大于0）。如果 `STRING` 非空，则返回真（true） |


**整数间的比较**
命令格式：

```sh
test $a -eq $b  # a是否等于b
```

| 测试参数 | 代表意义     |
| -------- | ------------ |
| -eq      | 是否相等     |
| -ne      | 是否不等于   |
| -gt      | 是否大于     |
| -lt      | 是否小于     |
| -ge      | 是否大于等于 |
| -le      | 是否小于等于 |

**字符串比较**

| 测试参数          |      | 代表意义                                              |
| ----------------- | ---- | ----------------------------------------------------- |
| test -z STRING    |      | 判断STRING是否为空，如果为空，则返回true              |
| test -n STRING    |      | 判断STRING是否非空，如果非空，则返回true（-n可以省略) |
| test str1 == str2 |      | 判断str1是否等于str2                                  |
| test str1 != str2 |      | 判断str1是否不等于str2                                |



**多重条件判定**
命令格式：

```sh
test -r filename -a -x filename
```

| 测试参数 | 代表意义                                            |
| -------- | --------------------------------------------------- |
| -a       | 两条件是否同时成立                                  |
| -o       | 两条件是否至少一个成立                              |
| !        | 取反。如 test ! -x file，当file不可执行时，返回true |

**判断符号[]**
[]与test用法几乎一模一样，更常用于if语句中。另外[[]]是[]的加强版，支持的特性更多。

例如：

```sh
[ 2 -lt 3 ]  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ [ -e test.sh ] && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ [ -e test2.sh ] && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

注意：

- []内的每一项都要用空格隔开
- 中括号内的变量，最好用双引号括起来
- 中括号内的常数，最好用单或双引号括起来

例如：

```sh
name="acwing yxc"
[ $name == "acwing yxc" ]  # 错误，等价于 [ acwing yxc == "acwing yxc" ]，参数太多
[ "$name" == "acwing yxc" ]  # 正确
```

##### 判断语句

**if…then形式**
类似于C/C++中的if-else语句。

**单层if**
命令格式：

```sh
if condition
then
    语句1
    语句2
    ...
fi
```

示例：

```sh
a=3
b=4

if [ "$a" -lt "$b" ] && [ "$a" -gt 2 ]
then
    echo ${a}在范围内
fi
输出结果：

3在范围内
```

**单层if-else**
命令格式

```sh
if condition
then
    语句1
    语句2
    ...
else
    语句1
    语句2
    ...
fi
```

示例：

```sh
a=3
b=4

if ! [ "$a" -lt "$b" ]
then
    echo ${a}不小于${b}
else
    echo ${a}小于${b}
fi
输出结果：

3小于4
```

**多层if-elif-elif-else**
命令格式

```sh
if condition
then
    语句1
    语句2
    ...
elif condition
then
    语句1
    语句2
    ...
elif condition
then
    语句1
    语句2
else
    语句1
    语句2
    ...
fi
```

示例：

```sh
a=4

if [ $a -eq 1 ]
then
    echo ${a}等于1
elif [ $a -eq 2 ]
then
    echo ${a}等于2
elif [ $a -eq 3 ]
then
    echo ${a}等于3
else
    echo 其他
fi
输出结果：

其他
```

**case…esac形式**
类似于C/C++中的switch语句。

命令格式

```sh
case $变量名称 in
    值1)
        语句1
        语句2
        ...
        ;;  # 类似于C/C++中的break
    值2)
        语句1
        语句2
        ...
        ;;
    *)  # 类似于C/C++中的default
        语句1
        语句2
        ...
        ;;
esac
```

示例：

```sh
a=4

case $a in
    1)
        echo ${a}等于1
        ;;  
    2)
        echo ${a}等于2
        ;;  
    3)                                                
        echo ${a}等于3
        ;;  
    *)
        echo 其他
        ;;  
esac
输出结果：

其他
```

##### 循环语句

**for…in…do…done**
命令格式：

```sh
for var in val1 val2 val3
do
    语句1
    语句2
    ...
done
```

示例1，输出a 2 cc，每个元素一行：

```sh
for i in a 2 cc
do
    echo $i
done
```

示例2，输出当前路径下的所有文件名，每个文件名一行：

```sh
for file in `ls`
do
    echo $file
done
```

示例3，输出1-10

```sh
for i in $(seq 1 10)
do
    echo $i
done
```

示例4，使用{1..10} 或者 {a..z}

```sh
for i in {a..z}
do
    echo $i
done
```

**for ((…;…;…)) do…done**
命令格式：

```sh
for ((expression; condition; expression))
do
    语句1
    语句2
done
```

示例，输出1-10，每个数占一行：

```sh
for ((i=1; i<=10; i++))
do
    echo $i
done
```

**while…do…done循环**
命令格式：

```sh
while condition
do
    语句1
    语句2
    ...
done
```

示例，文件结束符为Ctrl+d，输入文件结束符后read指令返回false。

```sh
while read name
do
    echo $name
done
```

**until…do…done循环**
当条件为真时结束。

命令格式：

```sh
until condition
do
    语句1
    语句2
    ...
done
```


示例，当用户输入yes或者YES时结束，否则一直等待读入。

```sh
until [ "${word}" == "yes" ] || [ "${word}" == "YES" ]
do
    read -p "Please input yes/YES to stop this program: " word
done
```

**break命令**
跳出当前一层循环，注意与C/C++不同的是：break不能跳出case语句。

示例

```sh
while read name
do
    for ((i=1;i<=10;i++))
    do
        case $i in
            8)
                break
                ;;
            *)
                echo $i
                ;;
        esac
    done
done
```

该示例每读入非EOF的字符串，会输出一遍1-7。
该程序可以输入Ctrl+d文件结束符来结束，也可以直接用Ctrl+c杀掉该进程。

**continue命令**
跳出当前循环。

示例：

```sh
for ((i=1;i<=10;i++))
do
    if [ `expr $i % 2` -eq 0 ]
    then
        continue
    fi
    echo $i
done
```

该程序输出1-10中的所有奇数。

**死循环的处理方式**
如果AC Terminal可以打开该程序，则输入Ctrl+c即可。

否则可以直接关闭进程：

1. 使用top命令找到进程的PID

2. 输入kill -9 PID即可关掉此进程



##### 函数

bash中的函数类似于C/C++中的函数，但return的返回值与C/C++不同，返回的是exit code，取值为0-255，0表示正常结束。

如果想获取函数的输出结果，可以通过echo输出到stdout中，然后通过$(function_name)来获取stdout中的结果。

函数的return值可以通过$?来获取。

命令格式：

```sh
[function] func_name() {  # function关键字可以省略
    语句1
    语句2
    ...
}
```

**不获取 return值和stdout值**
示例

```sh
func() {
    name=yxc
    echo "Hello $name"
}

func
```

输出结果：

```
Hello yxc
```

**获取 return值和stdout值**
不写return时，默认return 0。

示例

```sh
func() {
    name=yxc
    echo "Hello $name"

    return 123

}

output=$(func)
ret=$?

echo "output = $output"
echo "return = $ret"
```

输出结果：

```sh
output = Hello yxc
return = 123
```

**函数的输入参数**
在函数内，$1表示第一个输入参数，$2表示第二个输入参数，依此类推。

注意：函数内的$0仍然是文件名，而不是函数名。

示例：

```sh
func() {  # 递归计算 $1 + ($1 - 1) + ($1 - 2) + ... + 0
    word=""
    while [ "${word}" != 'y' ] && [ "${word}" != 'n' ]
    do
        read -p "要进入func($1)函数吗？请输入y/n：" word
    done

    if [ "$word" == 'n' ]
    then
        echo 0
        return 0
    fi  
    
    if [ $1 -le 0 ] 
    then
        echo 0
        return 0
    fi  
    
    sum=$(func $(expr $1 - 1))
    echo $(expr $sum + $1)

}

echo $(func 10)
```

输出结果：

```
55
```

**函数内的局部变量**
可以在函数内定义局部变量，作用范围仅在当前函数内。

可以在递归函数中定义局部变量。

命令格式：

```sh
local 变量名=变量值
```

例如：

```sh
#! /bin/bash

func() {
    local name=yxc
    echo $name
}
func

echo $name
```

输出结果：

```sh
yxc
```

第一行为函数内的name变量，第二行为函数外调用name变量，会发现此时该变量不存在。



##### exit

- exit命令用来退出当前shell进程，并返回一个退出状态；使用$?可以接收这个退出状态。

- exit命令可以接受一个整数值作为参数，代表退出状态。如果不指定，默认状态值是 0。

- exit退出状态只能是一个介于 0~255 之间的整数，其中只有 0 表示成功，其它值都表示失败。


示例：

创建脚本test.sh，内容如下：

```sh
#! /bin/bash

if [ $# -ne 1 ]  # 如果传入参数个数等于1，则正常退出；否则非正常退出。
then
    echo "arguments not valid"
    exit 1
else
    echo "arguments valid"
    exit 0
fi
```

执行该脚本：

```sh
acs@9e0ebfcd82d7:~$ chmod +x test.sh 
acs@9e0ebfcd82d7:~$ ./test.sh acwing
arguments valid
acs@9e0ebfcd82d7:~$ echo $?  # 传入一个参数，则正常退出，exit code为0
0
acs@9e0ebfcd82d7:~$ ./test.sh 
arguments not valid
acs@9e0ebfcd82d7:~$ echo $?  # 传入参数个数不是1，则非正常退出，exit code为1
1
```

return和exit的共同之处都是返回exit code，区别是return结束当前函数，exit结束整个shell脚本

##### 文件重定向

每个进程默认打开3个文件描述符：

- stdin标准输入，从命令行读取数据，文件描述符为0
- stdout标准输出，向命令行输出数据，文件描述符为1
- stderr标准错误输出，向命令行输出数据，文件描述符为2

可以用文件重定向将这三个文件重定向到其他文件中。

重定向命令列表
| 命令             | 说明                                  |
| ---------------- | ------------------------------------- |
| command > file   | 将stdout重定向到file中                |
| command < file   | 将stdin重定向到file中                 |
| command >> file  | 将stdout以追加方式重定向到file中      |
| command n> file  | 将文件描述符n重定向到file中           |
| command n>> file | 将文件描述符n以追加方式重定向到file中 |


**输入和输出重定向**

```sh
echo -e "Hello \c" > output.txt  # 将stdout重定向到output.txt中
echo "World" >> output.txt  # 将字符串追加到output.txt中

read str < output.txt  # 从output.txt中读取字符串

echo $str  # 输出结果：Hello World
```

**同时重定向stdin和stdout**
创建bash脚本：

```sh
#! /bin/bash

read a
read b

echo $(expr "$a" + "$b")
```
创建input.txt，里面的内容为：

```
3
4
```

执行命令：

```sh
acs@9e0ebfcd82d7:~$ chmod +x test.sh  # 添加可执行权限
acs@9e0ebfcd82d7:~$ ./test.sh < input.txt > output.txt  # 从input.txt中读取内容，将输出写入output.txt中
acs@9e0ebfcd82d7:~$ cat output.txt  # 查看output.txt中的内容
7
```



##### 引入外部脚本

类似于C/C++中的include操作，bash也可以引入其他文件中的代码。
引入的文件可以添加路径，比如使用绝对路径`source /home/acs/test1.sh`

语法格式：

```sh
. filename  # 注意点和文件名之间有一个空格

或

source filename
```

示例
创建test1.sh，内容为：

```sh
#! /bin/bash

name=yxc  # 定义变量name
```

然后创建test2.sh，内容为：

```sh
#! /bin/bash

source test1.sh # 或 . test1.sh

echo My name is: $name  # 可以使用test1.sh中的变量
```

执行命令：

```sh
acs@9e0ebfcd82d7:~$ chmod +x test2.sh 
acs@9e0ebfcd82d7:~$ ./test2.sh 
My name is: yxc
```



#### ssh

```shell
在本地.ssh文件中添加config文件
添加以下内容
Host myserver
	HostName 123.57.67.128
	User acs_9681
ssh-copy-id myserver 一键复制公钥到服务器
或者ssh-keygen本地生成密钥将本地密钥复制到服务器./.ssh/authorized_keys中

# 单引号中的$i可以求值
ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done'

# 双引号中的$i不可以求值 此处i不可被解析
ssh myserver "for ((i = 0; i < 10; i ++ )) do echo $i; done"

scp
命令格式：

scp source destination
将source路径下的文件复制到destination中

一次复制多个文件：

scp source1 source2 destination
复制文件夹：

scp -r ~/tmp myserver:/home/acs/
将本地家目录中的tmp文件夹复制到myserver服务器中的/home/acs/目录下。

scp -r ~/tmp myserver:homework/
将本地家目录中的tmp文件夹复制到myserver服务器中的~/homework/目录下。

scp -r myserver:homework .
将myserver服务器中的~/homework/文件夹复制到本地的当前路径下。

指定服务器的端口号：

scp -P 22 source1 source2 destination
注意： scp的-r -P等参数尽量加在source和destination之前。

使用scp配置其他服务器的vim和tmux
scp ~/.vimrc ~/.tmux.conf myserver:


```

#### git

##### 常用命令

```shell
git init #创建本地仓库
git add . #提交本地代码。注意这里只是添加进来，还没有完成提交
git commit -m "first_commit" #使用commit命令完成提交
接下来进行主机与github之间ssh连接
git remote add origin git@github.com:Donkeyzhenbang/git_test.git
#将创建好的本地仓库和云端仓库连接起来
git push -u origin master #将代码推送到云端
git checkout 13e23336bb0f5f5b6efce3ebe6f7366a8cba6fce #切换相应分支

git remote -v查看分支
git branch 查看分支
git branch -r查看远程分支
git checkout master切换分支
git branch temp创建新分支
git checkout -b 新分支名称

当切换到其他commit的时候 HEAD指针和master不指向同一个，无法在原分支上进行修改
此时可以先建一个分支再push上去


git diff readme.txt #查看文件和暂存区里面的文件区别
git restore --stage readme.txt #从暂存区拿出来
git rm --cached readme.txt #不再管理该文件
git status #查看git仓库状态

git rm --cached XX：将文件从仓库索引目录中删掉，不希望管理这个文件
git restore --staged xx：==将xx从暂存区里移除==
git checkout — XX或git restore XX：==将XX文件尚未加入暂存区的修改全部撤销==

1.2 git常用命令
git config --global user.name xxx：设置全局用户名，信息记录在~/.gitconfig文件中
git config --global user.email xxx@xxx.com：设置全局邮箱地址，信息记录在~/.gitconfig文件中
git init：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中
git add XX：将XX文件添加到暂存区
git add .：将所有待加入暂存区的文件加入暂存区
git rm --cached XX：将文件从仓库索引目录中删掉
git commit -m "给自己看的备注信息"：将暂存区的内容提交到当前分支
git status：查看仓库状态
git diff XX：查看XX文件相对于暂存区修改了哪些内容
git log：查看当前分支的所有版本
git reflog：查看HEAD指针的移动历史（包括被回滚的版本）
git reset --hard HEAD^ 或 git reset --hard HEAD~：将代码库回滚到上一个版本
git reset --hard HEAD^^：往上回滚两次，以此类推
git reset --hard HEAD~100：往上回滚100个版本
git reset --hard 版本号：回滚到某一特定版本
git checkout — XX或git restore XX：将XX文件尚未加入暂存区的修改全部撤销
git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库
git push -u (第一次需要-u以后不需要)：将当前分支推送到远程仓库
git push origin branch_name：将本地的某个分支推送到远程仓库
git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下
git checkout -b branch_name：创建并切换到branch_name这个分支
git branch：查看所有分支和当前所处分支
git checkout branch_name：切换到branch_name这个分支
git merge branch_name：将分支branch_name合并到当前分支上
git branch -d branch_name：删除本地仓库的branch_name分支
git branch branch_name：创建新分支
git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支
git push -d origin branch_name：删除远程仓库的branch_name分支
git pull：将远程仓库的当前分支与本地仓库的当前分支合并
git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并
git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应
git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地
git stash：将工作区和暂存区中尚未提交的修改存入栈中
git stash apply：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
git stash drop：删除栈顶存储的修改
git stash pop：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素
git stash list：查看栈中所有元素

#更改项目名称 更新远程仓库url
git remote set-url origin https://github.com/你的用户名/cpp_base_demo.git 

git rm -r --cached pics_test/ #删除远程仓库对应文件 适用于新添加.gitignore
```

![image-20240624184937380](C:\Users\30116\AppData\Roaming\Typora\typora-user-images\image-20240624184937380.png)

##### 问题处理

###### git clone 失败 

```shell
Windows
Cloning into bare repository 'bustub-public'...
fatal: unable to access 'https://github.com/cmu-db/bustub.git/': Failed to connect to github.com port 443 after 21135 ms: Timed out

解决方案git config --global http.sslVerify "false"

Ubuntu
fatal: 无法访问 'https://github.com/cmu-db/bustub.git/'：Could not resolve host: github.com
最终发现是ens33网卡找不到
```

![image-20240523175733014](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523175733014.png)

######  LF 将被 CRLF 替换

```shell
warning: server/qtmyserver/image_trans_server/mainwindow.ui 中的
在工作区中该文件仍保持原有的换行符
在ubuntu上，使用下面第一个方法解决问题
git config --global core.autocrlf input
windows使用下面命令
git config --global core.autocrlf false
```

![image-20240625150723483](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625150723483.png)

另外两种情况

![image-20240625151037206](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625151037206.png)

![image-20240625151116194](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240625151116194.png)

###### git权限问题

![image-20240708125304226](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240708125304226.png)

###### git远程新建分支

```sh
git push --set-upstream origin libmyjpeg
将本地分支和上游分支对应 以后可以直接使用git push
```

git合并分支

```sh
git init
git add .
git remote add origin git@github.com:Donkeyzhenbang/WebServer.git
git commit -m "server initial"

git branch --set-upstream-to=origin/master master
git pull origin master --allow-unrelated-histories #允许没有相关提交历史两个仓库pull push
```

![image-20240715194903659](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240715194903659.png)![image-20240715194956378](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240715194956378.png)

![image-20240715201625328](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240715201625328.png)

```sh
#本地有未保存的更改
git checkout -- webserver_days/day02/client   
git pull
```

1. **贮藏你的本地修改**：
   如果你暂时不想提交你的修改，但想先更新你的本地仓库以包含远程的最新更改，你可以使用 `git stash` 来贮藏你的修改。这会将你的工作区更改和暂存区的更改保存到一个“贮藏栈”中，然后你可以干净地拉取远程更改。拉取后，你可以使用 `git stash pop` 来恢复你的修改。

   ```bash
   git stash  
   git pull  
   git stash pop
   ```

   注意，如果贮藏和拉取后的更改在相同文件上发生冲突，你可能需要手动解决这些冲突。

2. **放弃你的本地修改**：
   如果你认为你的本地修改不重要，或者你只是想确保你的本地仓库与远程仓库同步，你可以简单地放弃这些修改：

   ```bash
   git checkout -- webserver_days/day02/client  
   git pull
   ```

   这个命令会用远程仓库的版本替换你本地文件的内容。

#### thrift

```sh
thrift安装
#wget下载thrift相关包
sudo wget -P /usr/local/src http://archive.apache.org/dist/thrift/0.16.0/thrift-0.16.0.tar.gz
#安装相关依赖
sudo apt install libboost-dev libboost-test-dev libboost-program-options-dev libevent-dev automake libtool flex bison pkg-config g++ libssl-dev
#运行configure
cd /usr/local/src/
sudo tar -xzvf thrift-0.16.0.tar.gz
cd thrift-0.16.0/
sudo ./configure
#编译安装
sudo make -j8
sudo make install
#查看版本
thrift -version
```



#### 管道/环境变量

```sh
find . -name '*.py' #查找当前目录下的py文件
find . -name '*.py' | xargs cat #将find出来的内容利用xargs将标准输出转化成文件参数 cat一下内容
find . -name '*.py' | xargs cat | wc -l #wc -l统计标准文件总行数
find ./code/cpp/vsc_test/ -name '*' | xargs wc -l #查找目录下所有文件 xargs也会输出总行数

#输出内容如下
wc: ./code/cpp/vsc_test/: Is a directory
      0 ./code/cpp/vsc_test/
      3 ./code/cpp/vsc_test/vsc.py
      4 ./code/cpp/vsc_test/test
      9 ./code/cpp/vsc_test/test.cpp
      1 ./code/cpp/vsc_test/temp.txt
     17 total
```

![image-20240630150450557](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630150450557.png)

概念
Linux系统中会用很多环境变量来记录配置信息。
环境变量类似于全局变量，可以被各个进程访问到。我们可以通过修改环境变量来方便地修改系统配置。

查看
列出当前环境下的所有环境变量：

```sh
env  # 显示当前用户的变量
set  # 显示当前shell的变量，包括当前用户的变量;
export  # 显示当前导出成用户变量的shell变量
```

输出某个环境变量的值：

```sh
echo $PATH
```

修改
环境变量的定义、修改、删除操作可以参考3. shell语法——变量这一节的内容。

为了将对环境变量的修改应用到未来所有环境下，可以将修改命令放到~/.bashrc文件中。
修改完~/.bashrc文件后，记得执行source ~/.bashrc，来将修改应用到当前的bash环境下。

为何将修改命令放到~/.bashrc，就可以确保修改会影响未来所有的环境呢？

每次启动bash，都会先执行~/.bashrc。
每次ssh登陆远程服务器，都会启动一个bash命令行给我们。
每次tmux新开一个pane，都会启动一个bash命令行给我们。
所以未来所有新开的环境都会加载我们修改的内容。
常见环境变量

```sh
HOME：用户的家目录。
PATH：可执行文件（命令）的存储路径。路径与路径之间用:分隔。当某个可执行文件同时出现在多个路径中时，会选择从左到右数第一个路径中的执行。下列所有存储路径的环境变量，均采用从左到右的优先顺序。
LD_LIBRARY_PATH：用于指定动态链接库(.so文件)的路径，其内容是以冒号分隔的路径列表。
C_INCLUDE_PATH：C语言的头文件路径，内容是以冒号分隔的路径列表。
CPLUS_INCLUDE_PATH：CPP的头文件路径，内容是以冒号分隔的路径列表。
PYTHONPATH：Python导入包的路径，内容是以冒号分隔的路径列表。
JAVA_HOME：jdk的安装目录。
CLASSPATH：存放Java导入类的路径，内容是以冒号分隔的路径列表。
```

短路原则 比如在多个文件内容中找到，会运行第一个找到的内容

![image-20240630152123484](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630152123484.png)

不同内容会用冒号隔开，这里`LD_LIBRARY_PATH`就是新添加一个路径然后将之前路径添加在后面用冒号隔开

![image-20240630152526708](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630152526708.png)

```sh
#修改echo $LD_LIBRARY_PATH中多余内容
检查~/.bashrc ~/.profile /etc/profile
删除其中相关MVS内容 
source命令无需sudo
source ~/.bashrc
source ~/.profile
source /ect/profile

$ echo $LD_LIBRARY_PATH
/home/jym/code/cpp/libjpeg-turbo/build:/opt/ros/noetic/lib
```



#### 服务器配置

```shell
usermod -aG sudo jym #分配sudo权限
adduser jym #创建普通用户
创建用户有两条命令：adduer和useradd，对应着两条删除用户的命令：deluser和userdel。
这两种命令之间的区别：
adduser：会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。
useradd：需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定shell版本。

利用vscode进行远程服务器开发
1.创建ssh链接 将id_rda.pub移至./authorized_keys
2.code连接上之后没有代码提示 需要在服务器端安装C/C++和python相关扩展插件

ssh-copy-id myserver #~/.ssh/config文件中需要先配置好 一键复制公钥到服务器
或者ssh-keygen本地生成密钥将本地密钥复制到服务器./.ssh/authorized_keys中
ssh-copy-id -i .ssh/id_rsa.pub jym@192.168.126.128
```



#### docker

##### 安装

解决docker安装时docker的包源download.docker.com地址一直无法访问问题 使用阿里云镜像源
1、检查环境
1.1 卸载旧版docker

```lua
sudo su
apt remove docker docker-engine docker.io containerd runc
```

2、安装依赖

```arduino
apt -y install ca-certificates curl gnupg lsb-release
```

3、添加密钥
输入命令

```awk
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

打印返回（返回OK即为成功）

```ebnf
ok
```

4、添加Docker软件源

```smali
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

5、安装docker

```vim
apt -y install docker-ce docker-ce-cli containerd.io
```

6、启动docker

```crmsh
systemctl start docker
```

7、查看docker状态

```ebnf
systemctl status docker
```

8、将当前用户添加到docker组
避免每次使用Docker时都需要使用sudo（默认情况下，只有root用户和docker组的用户才能运行Docker命令）

```crmsh
sudo usermod -aG docker $USER
```

10、重启docker服务

```ebnf
service docker restart
```

此外还需要配置镜像加速网址

```sh
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://9yejjmej.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

![image-20240630002430022](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240630002430022.png)

##### 常用命令

```sh
镜像
docker pull ubuntu:20.04#拉取一个镜像
docker images#列出本地所有镜像
docker image rm ubuntu:20.04 或 docker rmi ubuntu:20.04#删除镜像ubuntu:20.04
docker [container] commit CONTAINER IMAGE_NAME:TAG#创建某个container的镜像
docker save -o ubuntu_20_04.tar ubuntu:20.04#将镜像ubuntu:20.04导出到本地文件ubuntu_20_04.tar中
docker load -i ubuntu_20_04.tar #将镜像ubuntu:20.04从本地文件ubuntu_20_04.tar中加载出来

容器(container)
docker [container] create -it ubuntu:20.04：利用镜像ubuntu:20.04创建一个容器。
docker ps -a：查看本地的所有容器
docker [container] start CONTAINER：启动容器
docker [container] stop CONTAINER：停止容器
docker [container] restart CONTAINER：重启容器
docker [contaienr] run -itd ubuntu:20.04：创建并启动一个容器
docker [container] attach CONTAINER：进入容器
先按Ctrl-p，再按Ctrl-q可以挂起容器
docker [container] exec CONTAINER COMMAND：在容器中执行命令
docker [container] rm CONTAINER：删除容器
docker container prune：删除所有已停止的容器
docker export -o xxx.tar CONTAINER：将容器CONTAINER导出到本地文件xxx.tar中
docker import xxx.tar image_name:tag：将本地文件xxx.tar导入成镜像，并将镜像命名为image_name:tag
docker export/import与docker save/load的区别：
export/import会丢弃历史记录和元数据信息，仅保存容器当时的快照状态
save/load会保存完整记录，体积更大
docker top CONTAINER：查看某个容器内的所有进程
docker stats：查看所有容器的统计信息，包括CPU、内存、存储、网络等信息
docker cp xxx CONTAINER:xxx 或 docker cp CONTAINER:xxx xxx：在本地和容器间复制文件
docker rename CONTAINER1 CONTAINER2：重命名容器
docker update CONTAINER --memory 500MB：修改容器限制

实战
进入AC Terminal，然后：
scp /var/lib/acwing/docker/images/docker_lesson_1_0.tar server_name:  # 将镜像上传到自己租的云端服务器
ssh server_name  # 登录自己的云端服务器

docker load -i docker_lesson_1_0.tar  # 将镜像加载到本地
docker run -p 20000:22 --name my_docker_server -itd docker_lesson:1.0  # 创建并运行docker_lesson:1.0镜像

docker attach my_docker_server  # 进入创建的docker容器
passwd  # 设置root密码
去云平台控制台中修改安全组配置，放行端口20000。

返回AC Terminal，即可通过ssh登录自己的docker容器：

ssh root@xxx.xxx.xxx.xxx -p 20000  # 将xxx.xxx.xxx.xxx替换成自己租的服务器的IP地址
然后，可以仿照上节课内容，创建工作账户acs。

最后，可以参考4. ssh——ssh登录配置docker容器的别名和免密登录。
```



##### 问题处理

###### 避免sudo

```sh
sudo usermod -aG docker $USER
安装完docker，每次必须用sudo权限操作，比较麻烦。将docker加入sudo用户组，即可默认sudo权限运行，而不用sudo。具体步骤如下：
1、添加docker group
sudo groupadd docker
2、将当前用户添加到docker组
sudo gpasswd -a ${USER} docker
3、重启docker服务
sudo service docker restart
这样，再次使用docker命令，无需添加sudo。
```

###### 无法删除hello-world镜像

`Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container 6bf7c8fb514f is using its referenced image bf756fb1ae65`

```sh
出现这个问题的原因是删除的这个images可能被依赖与其他的container 解决方案如下:
docker ps -a #查询容器 结果如下
CONTAINER ID        IMAGE               COMMAND                  CREATED 
6bf7c8fb514f        hello-world         "/hello"                 3 days ago
docker rm 6bf7c8fb514f#删除对应的container容器即可
```

###### ssh访问拒绝

ssh: connect to host ip地址 port 20000: Connection refused
重启服务器/newgrp docker/打开服务器20000端口

###### Ubuntu docker

新建docker容器container的时候是要求有前台程序的 否则会自动退出
解决办法：-itd it表示交互模式 d表示后台运行
`docker run -p 20000:22 --name my_docker_server -itd` 
这句话意思是新建并运行一个docker容器用20000端口号映射22端口号 --name后面接的是容器名称 

![image-20240701165759393](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240701165759393.png)



Ubuntu新建docker latest版本容器，新建成功但是无法ssh远程20000端口号

```sh
apt install -y openssh-server
apt install vim
# 编辑 SSH 配置文件
vim /etc/ssh/sshd_config

# 确保以下行未被注释，或者添加/修改这些行
PermitRootLogin yes
PasswordAuthentication yes

# 设置 root 用户的密码
passwd root

# 启动 SSH 服务
service ssh start
```



# 嵌入式linux

## linux编程

### 消息队列

**2.1 mq_open()**
打开一个已存在的消息队列，或创建一个新的消息队列。

```c
mqd_t mq_open(const char *name, int oflag, ...);
```

入参：

- name：消息队列的名称。
- oflag：打开或创建队列时的选项标志，可以是以下一个或多个选项的按位或：
	- O_RDONLY：以只读方式打开消息队列。
	- O_WRONLY：以只写方式打开消息队列。
	- O_CREAT：如果指定的消息队列不存在，则创建它。
	- O_EXCL：与O_CREAT一起使用，如果消息队列已存在，则返回错误。
- 如果使用了O_CREAT标志，还需要提供两个额外的参数：mode_t mode和struct mq_attr *attr。mode指定新队列的权限，attr指定队列的属性；否则，这两个参数可以省略。

返回值：

- 成功时返回一个消息队列描述符（mqd_t类型），失败时返回-1并设置errno。

**2.2 mq_send() 和 mq_timedsend()**
向指定的消息队列发送消息。

```
int mq_send(mqd_t mqdes, const char *msg_ptr, size_t msg_len, unsigned int msg_prio);
int mq_timedsend(mqd_t mqdes, const char *msg_ptr, size_t msg_len, unsigned int msg_prio, const struct timespec *abs_timeout);
```

入参：

- mqdes: 消息队列描述符，通过 mq_open 函数打开消息队列后获得。

- msg_ptr: 指向要发送的消息的指针。
- msg_len: 要发送的消息的长度。
- msg_prio: 消息的优先级。
- abs_timeout: 指定发送消息的超时时间，为绝对时间，使用 struct timespec 结构表示。

返回值：

- 成功时，返回0表示消息成功发送。
- 失败时，返回-1，并设置 errno 以指示错误类型。

**2.3 mq_receive() 和 mq_timedreceive()**
从指定的消息队列接收消息。

```c
ssize_t mq_receive(mqd_t mqdes, char *msg_ptr, size_t msg_len, unsigned int *msg_prio);
ssize_t mq_timedreceive(mqd_t mqdes, char *msg_ptr, size_t msg_len, unsigned int *msg_prio, const struct timespec *abs_timeout);
```

入参：

- mqdes：消息队列描述符。

- msg_ptr：指向接收消息的缓冲区的指针。
- msg_len：接收消息的最大长度。
- msg_prio：指向存储接收消息优先级的变量的指针。
- abs_timeout（仅mq_timedreceive()）：指向timespec结构的指针，指定了接收操作的绝对超时时间；如果为NULL，则不设置超时。

返回值：

- 成功时返回接收到的消息长度，失败时返回-1并设置errno。

**2.4 mq_close()**
关闭一个打开的消息队列描述符。

```
int mq_close(mqd_t mqdes);
```

入参：

- mqdes：消息队列描述符。

返回值：

- 成功时返回0，失败时返回-1并设置errno。

**2.5 mq_unlink()**
删除一个命名的消息队列。

```
int mq_unlink(const char *name);
```

入参：

- name：要删除的消息队列的名称。

返回值：

- 成功时返回0，失败时返回-1并设置errno。

**2.6 mq_getattr() 和 mq_setattr()**

```
mq_getattr()获取消息队列的属性；mq_setattr()设置消息队列的属性，并可以选择性地获取旧的属性。

int mq_getattr(mqd_t mqdes, struct mq_attr *mqstat);
int mq_setattr(mqd_t mqdes, const struct mq_attr *mqstat, struct mq_attr *omqstat);
```

入参：

- mqdes：消息队列描述符。
- mqstat：指向mq_attr结构的指针，用于存储或设置消息队列的属性。
- omqstat（仅mq_setattr()）：如果非NULL，则在此结构中返回旧的队列属性。

返回值：

- 成功时返回0，失败时返回-1并设置errno。

# 问题解决

#### 相机软件移植

```bash
1.MVS
-海康相机驱动
-区分好ARM64和X86_64 不同架构安装包不同
-sudo dpkg -i MVS-2.1.2_aarch64_20221208.deb
-进入到/opt/MVS/bin 运行./MVS.sh

2.Galaxy
-大恒相机驱动
-sudo ./Galaxy_camera.run
-Galaxy_camera/bin/GalaxyView

3.SQlite
-嵌入式数据库管理系统
-sudo apt-get install sqlite3

4.qt5.12.8
-安装编译qt库和qtcreator
-连接HDMI显示屏
sudo apt-get install build-essential
sudo apt-get install qt5-default qtcreator


5.OpenCV4.5.4
---Ubuntu安装OpenCV4.5.4---

--下载源码 安装依赖--
-sudo apt-get install cmake
-sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev
-sudo apt-get install libgtk2.0-dev
-sudo apt-get install pkg-config

--编译安装--
-mkdir build
-cd build
-cmake -D WITH_TBB=ON -D WITH_EIGEN=ON -D OPENCV_GENERATE_PKGCONFIG=ON  -D BUILD_DOCS=ON -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF  -D WITH_OPENCL=OFF -D WITH_CUDA=OFF -D BUILD_opencv_gpu=OFF -D BUILD_opencv_gpuarithm=OFF -D BUILD_opencv_gpubgsegm=O -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
-make -j8
-sudo make install

--环境配置--
-sudo gedit /etc/ld.so.conf.d/opencv.conf
-/usr/local/lib
-sudo ldconfig 
-sudo gedit /etc/bash.bashrc
-PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
-export PKG_CONFIG_PATH
-source /etc/bash.bashrc
-sudo apt install mlocate
-sudo updatedb

--版本验证--
-pkg-config --modversion opencv4
-int main(int argc,char **argv)
-对应代码目录下运行./test lena.tif可以用这种方式验证图片


---Jetson nano安装OpenCV4.5.4---
--安装CMake和GCC
-sudo apt update
-sudo apt install cmake g++

--安装依赖项--
-sudo apt install build-essential cmake git pkg-config libgtk-3-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev gfortran openexr libatlas-base-dev python3-dev python3-numpy libtbb2 libtbb-dev libdc1394-22-dev

--下载opencv-4.5.4和opencv_contrib-4.5.4--
--修改文件夹名字 opencv opencv_contri放在opencv目录下--
-cd opencv
-mkdir build
-cd build

--cmake配置--
cmake -D CMAKE_BUILD_TYPE=RELEASE\
 -D CMAKE_INSTALL_PREFIX=/usr/local\
 -D OPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules\
 -D CUDA_ARCH_BIN='7.2'\
 -D WITH_CUDA=1\
 -D BUILD_opencv_python3=ON -DBUILD_opencv_python2=ON\
 -D WITH_V4L=ON\
 -D WITH_QT=ON\
 -D WITH_OPENGL=ON\
 -D CUDA_FAST_MATH=1\
 -D WITH_CUBLAS=1\
 -D OPENCV_GENERATE_PKGCONFIG=1\
 -D WITH_GTK_2_X=ON\
 -D WITH_GSTREAMER=ON ..

--编译安装 nano上大约要6-7个小时--
make
sudo make install

--版本验证 可以通过jtop查看opencv4.5.4带有CUDA加速--
-pkg-config --modversion opencv4



6.MobaXterm
-回调显示问题 
-suso su进入root权限
-sudo apt-get purge xauth清除.Xauthority
-sudo apt-get install xauth生成新的.Xauthority
-sudo cp /home/jym/.Xauthority /home/root/.Xauthority
```

![image-20240523160933360](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523160933360.png)

```
CUDNN
sudo apt-get install libfreeimage3 libfreeimage-dev
cd /usr/src/cudnn_samples_v7/mnistCUDNN #进入例子目录
sudo make #编译一下例子
sudo chmod a+x mnistCUDNN # 为可执行文件添加执行权限
./mnistCUDNN # 执行
```

```sh
关于galaxy MVS驱动首先创建目录
Galaxy海康 MVS大恒
~ImageMonitoring$ ls
bin Config pics
将Config_v1.db文件复制到Config文件夹

~/smy/image-monitoring$ ls
build.sh ImageMonitoring myGalaxy
执行build脚本

hik_mvs.pc放到/usr/lib/pkgconfigs

Galaxy/Galaxy_camera/lib/armv8 ls
GxGVTL.cti GxU3VTL.cti libgxiapi.so放到/usr/lib
Galaxy/Galaxy_camera/inc ls
DxImageProc.h GxIAPI.h放到/usr/include/
至此大恒驱动安装完成

opencv4.pc放到/usr/lib/pkgconfigs
~/smy/image-monitoring$不带sudo执行build脚本
生成的可执行文件 ~/ImageMonitoring/bin$ ls 
ImageMonitoring运行该可执行文件 (chmod +x example.sh)
执行该文件报错error while loading shared libraries: libopencv_core.so.4.5: cannot open shared object file: No such file or directory

解决方法如下:
sudo find / -name "libopencv_core.so.4.5"
sudo gedit /etc/ld.so.conf.d/opencv.conf

# 在文件末尾加上查询结果
/usr/local/opencv4.5.1/lib/libopencv_core.so.4.5

# 然后运行
sudo ldconfig 


ldd ./ImageMonitoring ldd用于打印程序或者库文件所依赖的共享库列表
```

#### 4G模块移植

```sh
$mkdir ../../kernel_out
$TEGRA_KERNEL_OUT=../../kernel_out
$export CROSS_COMPILE=aarch64-linux-gnu-
$sudo make ARCH=arm64 O=$TEGRA_KERNEL_OUT -j4
$sudo depmod -a
$sudo reboot
```

![image-20240523160945900](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523160945900.png)

#### Ubuntu找不到网卡

---

找不到ens33网卡时 只剩回环网卡

![image-20240523184241232](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240523184241232.png)

```shell
这里是重新安装无线网卡
auto ens33
iface ens33 inet dhcp
```

#### 阿里云服务器

##### 服务器断联

方法1：修改/etc/ssh/sshd_config文件
在云服务器上，打开/etc/ssh/sshd_config文件
找到如下两行：

#ClientAliveInterval0
#ClientAliveCountMax3
去掉注释，改成

ClientAliveInterval 30
ClientAliveCountMax 86400
![image-20240626095917981](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240626095917981.png)

##### socket端口访问不了

```
lsof -i:8888 #查看8888端口是否被使用
netstat -tnlp #查看服务器进程使用哪些端口

在agx上实验可以运行 最终发现是防火墙
https://blog.csdn.net/HHHSSD/article/details/117410122

首先将需要连接的端口加到安全组中 例如8888
！！！注意这里查看打开端口号才是正确的 
加完成后将该端口拉入防火墙
1.开启防火墙
systemctl start firewalld
2设置打开的端口号（永久打开）
firewall-cmd --add-port=8000/tcp --permanent
3.更新，端口的设置
firewall-cmd --reload
4.查看已经打开的端口
firewall-cmd --list-all

使用 netstat -tulpn 查看 端口使用情况
# 以8888端口为例
netstat -tulpn | grep 8888
```



#### 关于turbojpeg文件移植

```shell
1.首先需要在github上下载turbojpeg的库编译生成静态链接库 在makefile中链接上
2.下载turbojpeg.h头文件放到/usr/include保证系统能够索引到
3.动态库加载失败：cannot open shared object file: No such file or directory 链接库不全 使用ldd main 查看可执行文件链接库 
去~/.bashrc中export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/veroll/Linux/lesson1.6/library/lib
将冒号后面替换成需要路径 source ~/.bashrc即可
```



#### TCP程序打包

```shell
关于动态链接库除了在.bashrc中声明之外 还可以这样运行程序LD_LIBRARY_PATH=./lib/ ./test
添加用户sudo权限 usermod -aG sudo jym

QT打包程序 使用QCoreApplication直接打包完成的动态链接库链接添加好一些组件之后可以直接再另一个QT工程使用
如果需要在一个cpp工程中使用需要链接QT
```

qt打包动态链接库
![image-20240621173342577](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621173342577.png)

移植到AGX端的Makefile 注意qt5路径不一样
![image-20240621172928382](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621172928382.png)

g++链接时找不到跳过不兼容 原因是编译出来的库和链接库运行环境二者操作系统位数不统一
以后注意一定在同一个环境下进行编译链接！！！
![image-20240621173025296](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621173025296.png)



QT编译报错 parse error at "std"
![image-20240621170805587](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240621170805587.png)

！！！一定注意 生成的动态链接库和引用的动态链接库是否一致 在编译过程中 我遇到了bug 
因为之前sudo make install 在/usr/lib中已经有了libTCPClient，程序会自动索引
解决办法：删除/usr/lib中已经有的libTCPClient，编译程序重新生成 
运行时加上ldd这句话LD_LIBRARY_PATH=./lib/ ./bin/tcp

#### 运行和链接

经常遇到这个错误
`./bin/main: error while loading shared libraries: libmyjpeglib.so: cannot open shared object file: No such file or directory`
因为只在程序编译时链接了动态库，但是没告诉操作系统的动态链接器在程序运行时去哪里找动态库
加上这句话即可`LDFLAGS += -Wl,-rpath,./lib/` 

```sh
LDFLAGS := -L./lib/ -lmyjpeglib -lpng -ljpeg #-L链接的时候
LDFLAGS += -Wl,-rpath,./lib/ #运行时候
```

#### 开机自启流程

1.创建一个sh文件 `/usr/local/bin/hello.sh` 任意位置均可

```sh
#!/usr/bin/bash
# Simple program to use for testing startup configurations
# with systemd.
# By David Both
# Licensed under GPL V2
#
echo "###############################"
echo "######### Hello World! ########"
echo "###############################"
```

2.创建服务单元文件 `/etc/systemd/system/hello.service` 
相机程序启动失败即是因为系统环境变量与用户环境变量不一致
因此我们需要将用户环境变量导出，在服务中指定需要用的环境变量文件

```sh
# Simple service unit file to use for testing
# startup configurations with systemd.
# By David Both
# Licensed under GPL V2
#
[Unit]
Description=My hello shell script
After=network-online.target                                                                 
Wants=network-online.target #在网络稳定后在进行

[Service]
Type=oneshot
ExecStart=/usr/local/bin/hello.sh
#User=smy
#Group=smy
#Restart=on-failure
EnvironmentFile=/etc/systemd/system/mygalaxy.env#指定环境变量

[Install]
WantedBy=multi-user.target
```

 `systemctl start hello.service` 启动服务
`systemctl status hello.service` 查看服务状态
`systemctl enable hello.service`在启动序列中启用这个服务

3.重新加载 systemd 配置并启动服务
执行以下命令重新加载 systemd 配置并启动服务：

```sh
sudo systemctl daemon-reload
sudo systemctl start GalaxyWork.service
```

4.检查服务状态和日志

```sh
systemctl status GalaxyWork.service
journalctl -u GalaxyWork.service
```

通过设置环境变量文件并在服务文件中引用该文件，你可以确保服务在启动时拥有正确的环境变量。这样应该能够解决开机自启时遇到的问题。

#### 设置静态IP


1.使用图形化界面设置
![image-20240725165404282](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240725165404282.png)

在设置完静态IP之后，图形化界面记得设置为manual即可。
输入nmcli connection show MV424_2G4替换自己的网络即可。
静态网络不会有valid_lft和preferred_lft

![image-20240725172139726](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240725172139726.png)

![image-20240725180247241](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240725180247241.png)

2.使用nmcli设置

使用nmcli查看wifi进行连接

```sh
nmcli device wifi list
sudo nmcli device wifi connect <SSID> password <PASSWORD> #nmcli在终端连接wifi
nmcli device wifi disconnect #断开wifi
nmcli connection show #查看网络连接
nmcli device wifi rescan #重新扫描
sudo systemctl restart NetworkManager #重启服务
sudo nmcli connection delete <connection-name-or-uuid> #删除某个连接
```

1. **打开NetworkManager配置模式**：

   ```sh
   sudo nmcli networking off
   ```

2. **设置静态IP地址**： 

   使用 `nmcli` 设置 `wlan0` 的静态IP地址。替换 `<IP地址>`、`<子网掩码>`、`<网关>` 和 `<DNS服务器>` 为你的网络配置。这里避免产生多余的连接号

  ```sh
sudo nmcli con mod MV424_5G8 ipv4.addresses 10.0.0.109/24
sudo nmcli con mod MV424_5G8 ipv4.gateway 10.0.0.1
sudo nmcli con mod MV424_5G8 ipv4.dns 8.8.8.8
sudo nmcli con mod MV424_5G8 ipv4.method manual
  ```

3. **设置完成后left_time为forever**

   ![image-20240725182814475](https://adonkey.oss-cn-beijing.aliyuncs.com/picgo/image-20240725182814475.png)

4. **重新启用网络管理**：

   ```
   sudo nmcli networking on
   ```

5. **验证IP配置**： 使用 `ip addr` 命令来验证 `wlan0` 接口的IP地址配置：

   ```
   ip addr show wlan0
   ```

