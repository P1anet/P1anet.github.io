---
layout: post
title: "Linux高并发服务器开发"
subtitle: "Linux WebServer Development"
date: 2023-03-26
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags: 
  - LearnCS
---

# Prerequisites

> update-alternatives --config gcc //切换 gcc 版本

`$0` #当前执行的 shell script 文件名（带完整路径）
`$1 ~ $n` #依次存放 shell script 的命令行参数，数值大于 9 时必须要用`{}`括起来，比如`${10}`。命令行参数可以通过 shift 命令进行位移操作，位置参数根据 shift 命令指定的数值往前移动，如不指定移动值，则移动 1 次。例如:
`$*` #将所有命令行参数做为一个字符串存入此变量。
`$@` #将所有命令行参数做为一个字符串数组，每个参数为一个成员变量，存入此变量。
`$#` #命令行参数的个数。
`$?` #上一条命令执行后的返回码。
`$` #当前执行的 shell script 进程编号。
`$!` #上一个后台程序的进程编号。
`$_` #script 执行时，存放 bash 的绝对路径;bash 交互时，存放上一个命令最后一个命令行参数;邮件检测时，存放邮件文件名

## gcc/g++

GCC: GNU Compiler Collection. 包含了C, C++, Obejective-C, JAVA, Ada, GO等语言前端，也包括相应的库(libstdc++, libgcj等)

1. 工作流程
   - 源代码(`.h, .c, .cpp`)
   - ↓预处理器↓(头文件展开，宏替换，注释删除)
   - 预处理后源代码(`.i`)
   - ↓编译器↓
   - 汇编代码(`.s`)
   - ↓汇编器↓
   - 目标代码(`.o`)
   - ↓链接器↓(启动代码、库代码、其他目标代码)
   - 可执行程序(`.exe, .out`)

2. 编译选项
   - `-E`：预处理但不编译
   - `-S`：预处理、编译但不汇编
   - `-c`：预处理、编译、汇编但不链接
   - `-o`：指定输出文件名
   - `-I`：指定include包含文件的搜索目录
   - `-g`：在编译的时候生成调试信息，该程序可以被调试器调试
   - `-D`：在程序编译的时候指定一个宏（一般用于调试，在程序中使用`#ifdef 宏名`, `#endif`包围调试信息）
   - `-w`：不生成任何警告信息
   - `-Wall`：生成所有警告信息
   - `-On`：n的取值范围：0~3。编译器的优化选项的4个级别，-O0表示没有优化，-O1为缺省值，-O3优化级别最高。[更多](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)以及[说明](https://blog.csdn.net/qq_31108501/article/details/51842166)
   - `-l`：在程序编译的时候，指定使用的库
   - `-L`：指定编译的时候，搜索的库的路径
   - `-fPIC/fpic`：生成与位置无关的代码
   - `-shared`：生成共享目标文件，通常用在建立共享库时
   - `-std`：指定c方言，如：-std=C99，gcc默认的方言是GNU C

3. gcc和g++的区别
   - 对于.c代码，gcc将其视为c语言程序，g++将其视为c++程序
   - 对于.cpp代码，两者都视为c++程序（语法规则更严谨）
   - 在编译阶段，g++也调用gcc完成，但链接阶段gcc不能自动和c++库链接（可以用gcc -l stdc++）
   - 只有用gcc编译.c文件，__cplusplus宏（标志编译器是否按c++语法解释代码）才是未定义的

## 库

- 库文件是计算机上的一类文件，可以简单的把库文件看成一种代码仓库，它提供给使用者一些可以直接拿来用的变量、函数或类。
- 库是特殊的一种程序，编写库的程序和编写一般的程序区别不大，只是库不能单独运行。
- 库文件有两种，静态库和动态库（共享库），区别是：静态库在程序的链接阶段被复制到了程序中；动态库在链接阶段没有被复制到程序中，而是程序在运行时由系统动态加载到内存中供程序调用。
- 库的好处：1.代码保密 2.方便部署和分发
- 工作原理
  - 静态库．GCC进行链接时，会把静态库中代码打包到可执行程序中
  - 动态库．GCC进行链接时，动态库的代码不会被打包到可执行程序中
  - 程序启动之后，动态库会被动态加载到内存中，通过ldd(list dynamic dependencies)命令检查动态库依赖关系
- 静态库
  - 优点：加载速度快；发布程序时无需提供，移植方便
  - 缺点：消耗系统资源，浪费内存；更新、部署、发布麻烦
- 动态库
  - 优点：实现进程间资源共享；更新、部署、发布简单；可以控制何时加载动态库
  - 缺点：加载比静态库慢；发布程序时需要提供依赖的动态库

### 静态库

- 命名规则
  - Linux: libxxx.a
    - lib: 前缀（固定）
    - xxx: 库名，自定义
    - .a: 后缀（固定）
  - Windows: libxxx.lib
- 制作
  - 使用`gcc -c xxx.c`获得.o文件
  - 使用ar工具（archive）将.o文件打包
    - `ar rcs libxxx.a xxx.o xxx.o`
      - r - 将文件插入备存文件中
      - c - 建立备存文件
      - s - 索引
- 使用
  - `-L`指定库目录，`-l`指定库名称，即`libxxx.a`中的`xxx`部分，注意区分库名和库文件名

### 动态库

- 命名规则
  - Linux: libxxx.so
    - lib: 前缀（固定）
    - xxx: 库名，自定义
    - .so: 后缀（固定）
    - 在Linux下是一个可执行文件
  - Windows: libxxx.dll
- 制作
  - 使用`gcc -c -fpic/-fPIC xxx.c`获得.o文件，得到和位置无关的代码
  - 使用`gcc -shared xxx.o -o libxxx.so`获得动态库
- 使用
  - `ldd executable`列出可执行程序的动态库依赖关系
  - 如何定位共享库文件呢？
    - 当系统加载可执行代码时候，能够知道其所依赖的库的名字，但是还需要知道绝对路径。此时就需要系统的动态载入器来获取该绝对路径。
    - 对于elf格式的可执行程序，是由ld-linux.so来完成的，它先后搜索elf文件的`DT_RPATH`段 -> 环境变量`LD_LIBRARY_PATH` -> `/etc/ld.so.cache`文件列表 -> `/lib/`, `/usr/lib`目录找到库文件后将其载入内存。
  1. 配置环境变量`LD_LIBRARY_PATH`
  2. `vim /etc/profile` -> `source`
  3. `vim /etc/ld.so.conf` 将路径贴在文件下 -> `ldconfig`

## Makefile

见Makefile

## GDB调试

### 什么是GDB

GDB是由GNU软件系统社区提供的调试工具，同GCC配套组成了一套完整的开发环境，GDB是Linux和许多类Unix系统中的标准开发环境

一般来说，GDB主要帮助你完成下面四个方面的功能：
1. 启动程序，可以按照自定义的要求随心所欲的运行程序
2. 可让被调试的程序在所指定的调置的断点处停住（断点可以是条件表达式）
3. 当程序被停住时，可以检查此时程序中所发生的事
4. 可以改变程序，将一个BUG产生的影响修正从而测试其他BUG

### 准备工作

通常，在为调试而编译时我们会关掉编译器的优化选项（`-O`），并打开调试选项（`-g`）。另外，`-Wall`在尽量不影响程序行为的情况下选项打开所有warning，也可以发现许多问题，避免一些不必要的BUG

`gcc -g -Wall program.c -o program`

`-g`选项的作用是在可执行文件中加入源代码的信息比如可执行文件中第几条机器指令对应源代码的第几行，但并不是把整个源文件嵌入到可执行文件中，所以在调试时必须保证gdb能找到源文件。

### GDB命令

- 启动和退出
  - gdb 可执行程序
  - quit/q
- 给程序设置参数/获取设置参数
  - set args 10 20
  - show args
- 帮助
  - help
- 查看当前文件代码
  - list/l # 从默认位置显示
  - list/l 行号 # 从指定的行显示
  - list/l 函数名 # 从指定的函数显示
- 查看非当前文件代码
  - list/l 文件名:行号
  - list/l 文件名:函数名
- 设置显示的行数
  - show list/listsize
  - set list/listsize 行数
- 设置断点
  - b/break 行号
  - b/break 函数名
  - b/break 文件名：行号
  - b/break 文件名：函数
- 查看断点
  - i/info b/break
- 删除断点
  - d/del/delete 断点编号
- 设置断点无效
  - dis/disable 断点编号
- 设置断点生效
  - ena/enable 断点编号 
- 设置条件断点（一般用在循环的位置）
  - b/break 10 if i==5
- 运行GDB程序
  - start（程序停在第一行）
  - run（遇到断点才停）
- 继续运行，到下一个断点停
  - c/continue
- 向下执行一行代码（不会进入函数体）
  - n/next
- 向下单步调试（遇到函数进入函数体）
  - s/step
  - finish（跳出函数体）
- 变量操作
  - p/print 变量名（打印变量值）
  - ptype 变量名（打印变量类型）
- 自动变量操作
  - display 变量名（自动打印指定变量的值）
  - i/info display
  - undisplay 编号
- 其它操作
  - set var 变量名=变量值
  - until（跳出循环）
  - set print pretty
  - layout src (ctrl+x a)

## 文件IO

c库函数 fopen fclose fread fwrite fgets fputs fscanf fprintf fseek fgetc fputc ftell feof fflush

fopen返回值FILE* fp -> 数据结构{文件描述符，文件读写指针，I/O缓冲区}

文件描述符（整型值）：索引到对应的磁盘文件
文件读写指针（位置）：读写文件过程中指针的实际位置
I/O缓冲区（内存地址）：通过寻址找到对应的内存块