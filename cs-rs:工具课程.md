# cs-rs:工具课程

在学习计算机之前，事半功倍的一件事必须是熟练精通控制计算机的工具，为何？

试想你是否有以下经历？

- 在学习新语言时候第一步是用bash脚本做下载配置，看着符文一般命令很是破坏心情。
- 在你准备看名校AI或者系统课程时，第二课被大佬的VIM操作秀得更不上节奏，破坏了追课积极性。

no，

>  计算机设计的初衷就是任务自动化，然而学生们却常常陷在大量的重复任务中，或者无法完全发挥出诸如 版本控制、文本编辑器等工具的强大作用。效率低下和浪费时间还是其次，更糟糕的是，这还可能导致数据丢失或 无法完成某些特定任务。

从第一性原理思考工具的本质，上古工具必然也遵循cs知识，不妨一试。

## 命令行与shell

软件世界观：硬件 OS shell app。

如此，shell作为OS与app接口，自然需要学习，shell很多，旧必须选bash,rust后可选nushell替代。

bash同时也是一门C-like 脚本语言。脚本意味解释执行一段程序。即bash 是 bash lang的repl。命令无非是一段已经写好的程序可以执行一定任务。

go语言下查找重复行程序如下：

```go
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "strings"
)

func main() {
    counts := make(map[string]int)
    for _, filename := range os.Args[1:] {
        data, err := ioutil.ReadFile(filename)
        if err != nil {
            fmt.Fprintf(os.Stderr, "dup3: %v\n", err)
            continue
        }
        for _, line := range strings.Split(string(data), "\n") {
            counts[line]++
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}
```

bash语言脚本:

```bash
#!/bin/bash
set -u
set -e
set -o pipefail
# -u 防止引用为设置变量
# -e 命令失败 则立即退出
# -o pipe fail 
# Linux 下安装 Rust

curl –proto '=https' –tlsv1.2 -sSf https://sh.rustup.rs | sh

echo " " >> ~/.bashrc
echo "# Rust 语言环境变量" >> ~/.bashrc
echo "export RUSTPATH=$HOME/.cargo/bin" >> ~/.bashrc
echo "export PATH=$PATH:$RUSTPATH" >> ~/.bashrc
echo " " >> ~/.bashrc

source ~/.bashrc
```

windows下wsl2 + vscode打开连接后即可练习Linux下bash命令及脚本。

```bash
bash --version
date
echo "hello world"
echo $PATH
which python3
pwd
cd 
mkdir 
rm
rm -rf
ls
ls -h
ls --help
ls -l
mv
cp 
mkdir
man
echo hello > hello.txt
echo world >> hello.txt
cat hello.txt
echo $EPOCHSECONDS
ls -l | tail -n1
sudo apt install
curl
```

练习：

```
 cargo install tealdeer
```

tldr echo

https://www.shell-tips.com/

https://tldr.sh/ 查询curl使用

tldr chmod

tldr touch

完成MIT https://missing-semester-cn.github.io/第一课练习。

感谢rust 使用cargo轻易安装tldr，使用体验比npm 和Python生态好用。

有了tldr，再也不怕命令了。

## bash语言

环境配置是程序员必不可少的一项技能，能读懂并生成一些相当可读的脚本很重要。

实例：

```sh
#!/bin/bash

set -u
set -e
set -o pipefail

function is-even {
    if [[ $(($1 % 2)) -gt 0 ]]
    then return 1
    else return 0
    fi
}

function epoch {
    date +"%s"
}

if is-even $(epoch)
then echo "Even epoch"
else echo "Odd epoch"
fi

```

可见程序主要包含变量命名，函数定义，控制流等PL结构。

tips:

1. 写脚本要简单需要多写小函数。

2. 参数是基于位置的，通过 $1，$2... $n 访问。函数通过名称调用，参数通过空格分隔。

```bash
function my-function {
    local message=$1
    echo "Hello ${message}"
}

my-function "World"

#变量中捕获函数的输出或value
message=$(my-function "World")
echo ${message}

#声明和赋值分成两个独立的命令。
function my-function {
    local number
    number=$(command-that-might-fail)
    echo "Number is ${number}"
}

$@
$#
$?
$$
!!
$_

```

脚本最好表达式优先，控制流与数据分离。

由此不难完成简单脚本任务，当然，今天有nushell，Python等新工具可供选择。

后续任务： 

阅读nushell中文手册。

vim：

git

正则表达式：

- `.` 除换行符之外的”任意单个字符”
- `*` 匹配前面字符零次或多次
- `+` 匹配前面字符一次或多次
- `[abc]` 匹配 `a`, `b` 和 `c` 中的任意一个
- `(RX1|RX2)` 任何能够匹配`RX1` 或 `RX2`的结果
- `^` 行首
- `$` 行尾

数据处理awk 哲学：|pat match -> action

统计一下所有以`c` 开头，以 `e` 结尾，并且仅尝试过一次登陆的用户。

```
| awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
```

打印第四列

```
cat text.txt | awk '{print $4}'
```

Python分析

待续。









