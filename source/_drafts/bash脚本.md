---
title: bash脚本
tags:
  - bash
categories:
  - bash
date: 2024-11-21 10:14:22
---

Bash 是 Unix 系统和 Linux 系统的一种 Shell（命令行环境），是目前绝大多数 Linux 发行版的默认 Shell。

<!-- more -->

# shell

## shell 的含义

学习 Bash，首先需要理解 Shell 是什么。Shell 这个单词的原意是“外壳”，跟 kernel（内核）相对应，比喻内核外面的一层，即用户跟内核交互的对话界面。

具体来说，Shell 这个词有多种含义。

首先，Shell 是一个程序，提供一个与用户对话的环境。这个环境只有一个命令提示符，让用户从键盘输入命令，所以又称为命令行环境（command line interface，简写为 CLI）。Shell 接收到用户输入的命令，将命令送入操作系统执行，并将结果返回给用户。本书中，除非特别指明，Shell 指的就是命令行环境。

其次，Shell 是一个命令解释器，解释用户输入的命令。它支持变量、条件判断、循环操作等语法，所以用户可以用 Shell 命令写出各种小程序，又称为脚本（script）。这些脚本都通过 Shell 的解释执行，而不通过编译。

最后，Shell 是一个工具箱，提供了各种小工具，供用户方便地使用操作系统的功能。

## shell 的种类

Shell 有很多种，只要能给用户提供命令行环境的程序，都可以看作是 Shell。

历史上，主要的 Shell 有下面这些。

Bourne Shell（sh）
Bourne Again shell（bash）
C Shell（csh）
TENEX C Shell（tcsh）
Korn shell（ksh）
Z Shell（zsh）
Friendly Interactive Shell（fish）
Bash 是目前最常用的 Shell，除非特别指明，下文的 Shell 和 Bash 当作同义词使用，可以互换。

下面的命令可以查看当前设备的默认 Shell。

```bash
$ echo $SHELL
/bin/bash
```

下面的命令可以查看当前的 Linux 系统安装的所有 Shell。

```bash
$ cat /etc/shells
```

Linux 允许每个用户使用不同的 Shell，用户的默认 Shell 一般都是 Bash，或者与 Bash 兼容。

使用 chsh 命令，可以改变系统的默认 Shell。举例来说，要将默认 Shell 从 Bash 改成 Fish，首先要找出 Fish 可执行文件的位置。

```bash
$ which zsh
```

上面命令找出 zsh 可执行文件的位置，一般是/usr/bin/zsh。

然后，使用 chsh 命令切换默认 Shell。

```bash
$ chsh -s /usr/bin/zsh
```

上面命令会将当前的默认 Shell 改成 zsh

# 命令行环境

## 终端模拟器

所谓“终端模拟器”（terminal emulator）就是一个模拟命令行窗口的程序，让用户在一个窗口中使用命令行环境，并且提供各种附加功能，比如调整颜色、字体大小、行距等等。

用户可以安装第三方的终端程序。所有终端程序，尽管名字不同，基本功能都是一样的，就是让用户可以进入命令行环境，使用 Shell。

## 进入和退出

进入命令行环境以后，一般就已经打开 Bash 了。如果你的 Shell 不是 Bash，可以输入 bash 命令启动 Bash。

```bash
$ bash
```

退出 Bash 环境，可以使用 exit 命令，也可以同时按下 Ctrl + d。

```bash
$ exit
```

Bash 的基本用法就是在命令行输入各种命令，非常直观。

```bash
$ pwd
```

上述命令，会显示当前所在的目录。

如果不小心输入了 pwe，会返回一个提示，表示输入出错，没有对应的可执行程序。

```bash
$ pwe
bash: pwe：未找到命令
```

# 基本语法

## echo 命令

echo 命令的作用是在屏幕输出一行文本，可以将该命令的参数原样输出。

```bash
$ echo hello world
hello world
```

上面例子中，echo 的参数是 hello world，可以原样输出。

如果想要输出的是多行文本，即包括换行符。这时就需要把多行文本放在引号里面。

```bash
$ echo "<html lang="en">
  <head>
    <title>Document</title>
  </head>
  <body>
    <h1>Hello World</h1>
  </body>
</html>
"
```

上面例子中，echo 可以原样输出多行文本。

### -n 参数

默认情况下，echo 输出的文本末尾会有一个回车符。-n 参数可以取消末尾的回车符，使得下一个提示符紧跟在输出内容的后面。

```bash
$ echo -n hello world
hello world$
```

上面例子中，world 后面直接就是下一行的提示符$。

```bash
$ echo a;echo b
a
b

$ echo -n a;echo b
ab
```

上面例子中，-n 参数可以让两个 echo 命令的输出连在一起，出现在同一行。

### -e 参数

-e 参数会解释引号（双引号和单引号）里面的特殊字符（比如换行符\n）。如果不使用-e 参数，即默认情况下，引号会让特殊字符变成普通字符，echo 不解释它们，原样输出。

```bash
$ echo "Hello\nWorld"
Hello\nWorld

# 双引号的情况
$ echo -e "Hello\nWorld"
Hello
World

# 单引号的情况
$ echo -e 'Hello\nWorld'
Hello
World
```

上面代码中，-e 参数使得\n 解释为换行符，导致输出内容里面出现换行。

## 命令格式

命令行环境中，主要通过使用 Shell 命令，进行各种操作。Shell 命令基本都是下面的格式。

```bash
$ command [ arg1 ... [ argN ]]
```

上面代码中，command 是具体的命令或者一个可执行文件，arg1 ... argN 是传递给命令的参数，它们是可选的。

```bash
$ ls -l
```

上面这个命令中，ls 是命令，-l 是参数。

有些参数是命令的配置项，这些配置项一般都以一个连词线开头，比如上面的-l。同一个配置项往往有长和短两种形式，比如-l 是短形式，--list 是长形式，它们的作用完全相同。短形式便于手动输入，长形式一般用在脚本之中，可读性更好，利于解释自身的含义。

```bash
# 短形式
$ ls -r

# 长形式
$ ls --reverse
```

上面命令中，-r 是短形式，--reverse 是长形式，作用完全一样。前者便于输入，后者便于理解。

Bash 单个命令一般都是一行，用户按下回车键，就开始执行。有些命令比较长，写成多行会有利于阅读和编辑，这时可以在每一行的结尾加上反斜杠，Bash 就会将下一行跟当前行放在一起解释。

```bash
$ echo foo bar

# 等同于
$ echo foo \
bar
```

## 空格

Bash 使用空格（或 Tab 键）区分不同的参数。

```bash
$ command foo bar
```

上面命令中，foo 和 bar 之间有一个空格，所以 Bash 认为它们是两个参数。

如果参数之间有多个空格，Bash 会自动忽略多余的空格。

```bash
$ echo this is a     test
this is a test
```

上面命令中，a 和 test 之间有多个空格，Bash 会忽略多余的空格。

## 分号

分号（;）是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令。

```bash
$ clear; ls
```

上面例子中，Bash 先执行 clear 命令，执行完成后，再执行 ls 命令。

注意，使用分号时，第二个命令总是接着第一个命令执行，不管第一个命令执行成功或失败。

## 命令的组合符 && 和 ||

除了分号，Bash 还提供两个命令组合符&&和||，允许更好地控制多个命令之间的继发关系。

```bash
Command1 && Command2
```

上面命令的意思是，如果 Command1 命令运行成功，则继续运行 Command2 命令。

```bash
Command1 || Command2
```

上面命令的意思是，如果 Command1 命令运行失败，则继续运行 Command2 命令。

下面是一些例子。

```bash
$ cat filelist.txt ; ls -l filelist.txt
```

上面例子中，只要 cat 命令执行结束，不管成功或失败，都会继续执行 ls 命令。

```bash
$ cat filelist.txt && ls -l filelist.txt
```

上面例子中，只有 cat 命令执行成功，才会继续执行 ls 命令。如果 cat 执行失败（比如不存在文件 flielist.txt），那么 ls 命令就不会执行。

```bash
$ mkdir foo || mkdir bar
```

上面例子中，只有 mkdir foo 命令执行失败（比如 foo 目录已经存在），才会继续执行 mkdir bar 命令。如果 mkdir foo 命令执行成功，就不会创建 bar 目录了。

## type 命令

Bash 本身内置了很多命令，同时也可以执行外部程序。怎么知道一个命令是内置命令，还是外部程序呢？

type 命令用来判断命令的来源。

```bash
$ type echo
echo is a shell builtin
$ type ls
ls is hashed (/bin/ls)
```

上面代码中，type 命令告诉我们，echo 是内部命令，ls 是外部程序（/bin/ls）。

type 命令本身也是内置命令。

```bash
$ type type
type is a shell builtin
```

如果要查看一个命令的所有定义，可以使用 type 命令的-a 参数。

```bash
$ type -a echo
echo is shell builtin
echo is /usr/bin/echo
echo is /bin/echo
```

上面代码表示，echo 命令既是内置命令，也有对应的外部程序。

type 命令的-t 参数，可以返回一个命令的类型：别名（alias），关键词（keyword），函数（function），内置命令（builtin）和文件（file）。

```bash
$ type -t bash
file
$ type -t if
keyword
```

上面例子中，bash 是文件，if 是关键词。

## 快捷键

Bash 提供很多快捷键，可以大大方便操作。下面是一些最常用的快捷键

- Ctrl + L：清除屏幕并将当前行移到页面顶部。
- Ctrl + C：中止当前正在执行的命令。
- Shift + PageUp：向上滚动。
- Shift + PageDown：向下滚动。
- Ctrl + U：从光标位置删除到行首。
- Ctrl + K：从光标位置删除到行尾。
- Ctrl + W：删除光标位置前一个单词。
- Ctrl + D：关闭 Shell 会话。
- ↑，↓：浏览已执行命令的历史记录。

# 模式扩展

Shell 接收到用户输入的命令以后，会根据空格将用户的输入，拆分成一个个词元（token）。然后，Shell 会扩展词元里面的特殊字符，扩展完成后才会调用相应的命令。

这种特殊字符的扩展，称为模式扩展（globbing）。其中有些用到通配符，又称为通配符扩展（wildcard expansion）。Bash 一共提供八种扩展。

- 波浪线扩展
- ? 字符扩展
- 字符扩展
- 方括号扩展
- 大括号扩展
- 变量扩展
- 子命令扩展
- 算术扩展

Bash 是先进行扩展，再执行命令。因此，扩展的结果是由 Bash 负责的，与所要执行的命令无关。命令本身并不存在参数扩展，收到什么参数就原样执行。这一点务必需要记住。

模式扩展与正则表达式的关系是，模式扩展早于正则表达式出现，可以看作是原始的正则表达式。它的功能没有正则那么强大灵活，但是优点是简单和方便。

Bash 允许用户关闭扩展。

```bash
$ set -o noglob
# 或者
$ set -f
```

下面的命令可以重新打开扩展。

```bash
$ set +o noglob
# 或者
$ set +f
```

## 波浪扩展

波浪线~会自动扩展成当前用户的主目录。

```bash
$ echo ~
/home/me
```

~/dir 表示扩展成主目录的某个子目录，dir 是主目录里面的一个子目录名。

```bash
# 进入 /home/me/foo 目录
$ cd ~/foo
```

~user 表示扩展成用户 user 的主目录。

```bash
$ echo ~foo
/home/foo

$ echo ~root
/root
```

上面例子中，Bash 会根据波浪号后面的用户名，返回该用户的主目录。

如果~user 的 user 是不存在的用户名，则波浪号扩展不起作用。

```bash
$ echo ~nonExistedUser
~nonExistedUser
```

~+会扩展成当前所在的目录，等同于 pwd 命令。

```bash
$ cd ~/foo
$ echo ~+
/home/me/foo
```

## ? 字符扩展

?字符代表文件路径里面的任意单个字符，不包括空字符。比如，Data???匹配所有 Data 后面跟着三个字符的文件名。

```bash
# 存在文件 a.txt 和 b.txt
$ ls ?.txt
a.txt b.txt
```

上面命令中，?表示单个字符，所以会同时匹配 a.txt 和 b.txt。

如果匹配多个字符，就需要多个?连用。

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls ??.txt
ab.txt
```

上面命令中，??匹配了两个字符。

? 字符扩展属于文件名扩展，只有文件确实存在的前提下，才会发生扩展。如果文件不存在，扩展就不会发生。

```bash
# 当前目录有 a.txt 文件
$ echo ?.txt
a.txt

# 当前目录为空目录
$ echo ?.txt
?.txt
```

上面例子中，如果?.txt 可以扩展成文件名，echo 命令会输出扩展后的结果；如果不能扩展成文件名，echo 就会原样输出?.txt。

## \* 字符扩展

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls *.txt
a.txt b.txt ab.txt
```

上面例子中，\*.txt 代表后缀名为.txt 的所有文件。

如果想输出当前目录的所有文件，直接用\*即可。

```bash
$ ls *
```

\*可以匹配空字符，下面是一个例子。

```bash
# 存在文件 a.txt、b.txt 和 ab.txt
$ ls a*.txt
a.txt ab.txt

$ ls *b*
b.txt ab.txt
```

注意，*不会匹配隐藏文件（以.开头的文件），即 ls *不会输出隐藏文件。

如果要匹配隐藏文件，需要写成.\*。

```bash
# 显示所有隐藏文件
$ echo .*
```

如果要匹配隐藏文件，同时要排除.和..这两个特殊的隐藏文件，可以与方括号扩展结合使用，写成.[!.]\*。

```bash
$ echo .[!.]*
```

注意，\*字符扩展属于文件名扩展，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。

```bash
# 当前目录不存在 c 开头的文件
$ echo c*.txt
c*.txt
```

上面例子中，当前目录里面没有 c 开头的文件，导致 c\*.txt 会原样输出。

\*只匹配当前目录，不会匹配子目录。

```bash
# 子目录有一个 a.txt
# 无效的写法
$ ls *.txt

# 有效的写法
$ ls */*.txt
```

上面的例子，文本文件在子目录，_.txt 不会产生匹配，必须写成_/\*.txt。有几层子目录，就必须写几层星号。

## 方括号扩展

方括号扩展的形式是[...]，只有文件确实存在的前提下才会扩展。如果文件不存在，就会原样输出。括号之中的任意一个字符。比如，[aeiou]可以匹配五个元音字母中的任意一个。

```bash
# 存在文件 a.txt 和 b.txt
$ ls [ab].txt
a.txt b.txt

# 只存在文件 a.txt
$ ls [ab].txt
a.txt
```

上面例子中，[ab]可以匹配 a 或 b，前提是确实存在相应的文件。

方括号扩展属于文件名匹配，即扩展后的结果必须符合现有的文件路径。如果不存在匹配，就会保持原样，不进行扩展。

```bash
# 不存在文件 a.txt 和 b.txt
$ ls [ab].txt
ls: 无法访问'[ab].txt': 没有那个文件或目录
```

上面例子中，由于扩展后的文件不存在，[ab].txt 就原样输出了，导致 ls 命名报错。

方括号扩展还有两种变体：[^...]和[!...]。它们表示匹配不在方括号里面的字符，这两种写法是等价的。比如，[^abc]或[!abc]表示匹配除了 a、b、c 以外的字符。

```bash
# 存在 aaa、bbb、aba 三个文件
$ ls ?[!a]?
aba bbb
```

上面命令中，[!a]表示文件名第二个字符不是 a 的文件名，所以返回了 aba 和 bbb 两个文件。

注意，如果需要匹配[字符，可以放在方括号内，比如[[aeiou]。如果需要匹配连字号-，只能放在方括号内部的开头或结尾，比如[-aeiou]或[aeiou-]。

## [start-end] 扩展

方括号扩展有一个简写形式[start-end]，表示匹配一个连续的范围。比如，[a-c]等同于[abc]，[0-9]匹配[0123456789]。

```bash
# 存在文件 a.txt、b.txt 和 c.txt
$ ls [a-c].txt
a.txt
b.txt
c.txt

# 存在文件 report1.txt、report2.txt 和 report3.txt
$ ls report[0-9].txt
report1.txt
report2.txt
report3.txt
...
```

下面是一些常用简写的例子。

- [a-z]：所有小写字母。
- [a-zA-Z]：所有小写字母与大写字母。
- [a-zA-Z0-9]：所有小写字母、大写字母与数字。
- [abc]\*：所有以 a、b、c 字符之一开头的文件名。
- program.[co]：文件 program.c 与文件 program.o。
- BACKUP.[0-9][0-9][0-9]：所有以 BACKUP.开头，后面是三个数字的文件名。

这种简写形式有一个否定形式[!start-end]，表示匹配不属于这个范围的字符。比如，[!a-zA-Z]表示匹配非英文字母的字符。

```bash
$ ls report[!1–3].txt
report4.txt report5.txt
```

上面代码中，[!1-3]表示排除 1、2 和 3。

## 打括号扩展

大括号扩展{...}表示分别扩展成大括号里面的所有值，各个值之间使用逗号分隔。比如，{1,2,3}扩展成 1 2 3。

```bash
$ echo {1,2,3}
1 2 3

$ echo d{a,e,i,u,o}g
dag deg dig dug dog

$ echo Front-{A,B,C}-Back
Front-A-Back Front-B-Back Front-C-Back
```

注意，大括号扩展不是文件名扩展。它会扩展成所有给定的值，而不管是否有对应的文件存在。

```bash
$ ls {a,b,c}.txt
ls: 无法访问'a.txt': 没有那个文件或目录
ls: 无法访问'b.txt': 没有那个文件或目录
ls: 无法访问'c.txt': 没有那个文件或目录
```

上面例子中，即使不存在对应的文件，{a,b,c}依然扩展成三个文件名，导致 ls 命令报了三个错误。

另一个需要注意的地方是，大括号内部的逗号前后不能有空格。否则，大括号扩展会失效。

```bash
$ echo {1 , 2}
{1 , 2}
```

上面例子中，逗号前后有空格，Bash 就会认为这不是大括号扩展，而是三个独立的参数。

逗号前面可以没有值，表示扩展的第一项为空。

```bash
$ cp a.log{,.bak}

# 等同于
# cp a.log a.log.bak
```

大括号可以嵌套。

```bash
$ echo {j{p,pe}g,png}
jpg jpeg png

$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b
```

大括号也可以与其他模式联用，并且总是先于其他模式进行扩展。

```bash
$ echo /bin/{cat,b*}
/bin/cat /bin/b2sum /bin/base32 /bin/base64 ... ...

# 基本等同于
$ echo /bin/cat;echo /bin/b*
```

上面例子中，会先进行大括号扩展，然后进行\*扩展，等同于执行两条 echo 命令。

大括号可以用于多字符的模式，方括号不行（只能匹配单字符）。

```bash
$ echo {cat,dog}
cat dog
```

由于大括号扩展{...}不是文件名扩展，所以它总是会扩展的。这与方括号扩展[...]完全不同，如果匹配的文件不存在，方括号就不会扩展。这一点要注意区分。

```bash
# 不存在 a.txt 和 b.txt
$ echo [ab].txt
[ab].txt

$ echo {a,b}.txt
a.txt b.txt
```

上面例子中，如果不存在 a.txt 和 b.txt，那么[ab].txt 就会变成一个普通的文件名，而{a,b}.txt 可以照样扩展。

## {start..end} 扩展

大括号扩展有一个简写形式{start..end}，表示扩展成一个连续序列。比如，{a..z}可以扩展成 26 个小写英文字母。

```bash
$ echo {a..c}
a b c

$ echo d{a..d}g
dag dbg dcg ddg

$ echo {1..4}
1 2 3 4

$ echo Number_{1..5}
Number_1 Number_2 Number_3 Number_4 Number_5
```

这种简写形式支持逆序。

```bash
$ echo {c..a}
c b a

$ echo {5..1}
5 4 3 2 1
```

注意，如果遇到无法理解的简写，大括号模式就会原样输出，不会扩展。

```bash
$ echo {a1..3c}
{a1..3c}
```

这种简写形式可以嵌套使用，形成复杂的扩展。

```bash
$ echo .{mp{3..4},m4{a,b,p,v}}
.mp3 .mp4 .m4a .m4b .m4p .m4v
```

大括号扩展的常见用途为新建一系列目录。

```bash
$ mkdir {2007..2009}-{01..12}
```

上面命令会新建 36 个子目录，每个子目录的名字都是”年份-月份“。

这个写法的另一个常见用途，是直接用于 for 循环。

```bash
for i in {1..4}
do
  echo $i
done
```

上面例子会循环 4 次。

如果整数前面有前导 0，扩展输出的每一项都有前导 0。

```bash
$ echo {01..5}
01 02 03 04 05

$ echo {001..5}
001 002 003 004 005
```

这种简写形式还可以使用第二个双点号（start..end..step），用来指定扩展的步长。

```bash
$ echo {0..8..2}
0 2 4 6 8
```

上面代码将 0 扩展到 8，每次递增的长度为 2，所以一共输出 5 个数字。

多个简写形式连用，会有循环处理的效果。

```bash
$ echo {a..c}{1..3}
a1 a2 a3 b1 b2 b3 c1 c2 c3
```

## 变量扩展

Bash 将美元符号$开头的词元视为变量，将其扩展成变量值。

```bash
$ echo $SHELL
/bin/bash
```

变量名除了放在美元符号后面，也可以放在${}里面。

```bash
$ echo ${SHELL}
/bin/bash
```

${!string*}或${!string@}返回所有匹配给定字符串 string 的变量名。

```bash
$ echo ${!S*}
SECONDS SHELL SHELLOPTS SHLVL SSH_AGENT_PID SSH_AUTH_SOCK
```

上面例子中，${!S\*}扩展成所有以 S 开头的变量名。

## 子命令扩展

`$(...)` 可以扩展成另一个命令的运行结果，该命令的所有输出都会作为返回值。

```bash
$ echo $(date)
Tue Jan 28 00:01:13 CST 2020
```

上面例子中，$(date)返回 date 命令的运行结果。

还有另一种较老的语法，子命令放在反引号之中，也可以扩展成命令的运行结果。

```bash
$ echo `date`
Tue Jan 28 00:01:13 CST 2020
```

`$(...)`可以嵌套，比如$(ls $(pwd))。
