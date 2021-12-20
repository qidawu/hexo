---
title: GUN/Bash 系列（五）Shell 内建命令总结
date: 2015-03-15 11:27:26
updated:
tags: GNU/Linux
---

GUN/Bash 提供了一些内建命令 (BUILTIN COMMANDS)，用于在命令行上方便使用：

# 常用类

`echo` 显示一行文本或变量

`unset` 取消变量

`set` 查看所有变量（环境变量&用户变量）

`env` 查看所有环境变量（格式好看些）

`export`

* 查看所有环境变量
* 将局部变量转成环境变量：
  * 可以利用 `export` 命令将局部变量转为环境变量，但是用户注销时值将丢失；
  * 环境配置文件中，经常会用到 `export` 命令，相当于每次登录时系统都帮用户 `export` 一下所需环境变量；
  * 环境变量在当前进程 fork 出来的子进程中也能被访问到；
  * 目前发现安装软件时有用。

`source` 或 `.`

* 加载环境配置文件（无须 `exit` 注销）
* 执行脚本（在父进程bash中执行，设置的变量都会保留）



`declare` 或 `typeset`

* `-a` 定义数组类型
* `-i` 定义整数类型
* `-x` 将用户变量转成环境变量（与 `export` 一样）
* `+x` 将环境变量降为用户变量
* `-r` 定义 readonly 类型

`read` 读取来自键盘输入的变量

* `-p` 后接提示符
* `-t` 后接等待“秒数”

`test`

- `-e` 该文件名是否存在（exist）
- `-s` 该文件大小是否非 0
- `-z` 是否为空字符串（zero）
- `-f` 是否为文件（file）
- `-d` 是否为目录（directory）
- `-b` 是否为块特殊文件（block）
- `-L` 是否为连接文件（link）
- `-r` `-w` `-x` 是否可读、可写、可执行
- `-a` `-o` `!` 且、或、非
- `-eq` `-ne` `-gt` `-lt` `-ge` `-le` （判断2个整数）相等、不等、大于、小于、大于等于、小于等于

`[]`

* 中括号 `[]` 的使用方法与 `test` 命令几乎一模一样，只是中括号常用于条件判断式 `if…then…fi`
* 中括号内的每个元素，都要有**空格符**分隔
* 中括号内的变量，最好都以**双引号**括起来
* 中括号内的常量（字符串），最好都以单引号 `''` 或双引号 `""` 括起来



`sh` 以 `sh` 方式执行，至少需要 `r` 权限；若以绝对路径方式执行，则需要 `r` 与 `x` 权限

* `-n` 不执行 script，仅验证语法。若语法无误，则不显示任何信息。（貌似仅能验证关键字错误？）
* `-v` 在执行 script 前，先将 script 的内容输出到屏幕上
* `-x` 将 script 执行过程逐步输出到屏幕上

# 作业控制类

Bash 是一个多任务的 CLI ，有以下作业控制（Job Control）相关的命令：

| 命令     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| `jobs`   | 显示（当前会话中的）后台作业表                               |
| `fg`     | 将后台作业调到前台执行（前台运行作业）                       |
| `bg`     | 继续执行指定的后台作业（后台运行作业）                       |
| `Ctrl+Z` | 暂停/挂起目前的命令，转入后台运行。通过在命令后追加一个&，可以将该命令转入后台运行 |
| `Ctrl+C` | 终止目前的命令                                               |

![more bash tricks](https://wizardzines.com/comics/more-bash-tricks/more-bash-tricks.png)

# 参考

http://www.gnu.org/software/bash/manual/bashref.html#Shell-Builtin-Commands

![builtins](https://wizardzines.com/comics/builtins/builtins.png)