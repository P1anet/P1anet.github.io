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

# Linux高并发服务器开发

## Prerequisites

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

### gcc/g++

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

### 库

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

#### 静态库

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

#### 动态库

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

### Makefile

见Makefile

### GDB调试

#### 什么是GDB

GDB是由GNU软件系统社区提供的调试工具，同GCC配套组成了一套完整的开发环境，GDB是Linux和许多类Unix系统中的标准开发环境

一般来说，GDB主要帮助你完成下面四个方面的功能：
1. 启动程序，可以按照自定义的要求随心所欲的运行程序
2. 可让被调试的程序在所指定的调置的断点处停住（断点可以是条件表达式）
3. 当程序被停住时，可以检查此时程序中所发生的事
4. 可以改变程序，将一个BUG产生的影响修正从而测试其他BUG

#### 准备工作

通常，在为调试而编译时我们会关掉编译器的优化选项（`-O`），并打开调试选项（`-g`）。另外，`-Wall`在尽量不影响程序行为的情况下选项打开所有warning，也可以发现许多问题，避免一些不必要的BUG

`gcc -g -Wall program.c -o program`

`-g`选项的作用是在可执行文件中加入源代码的信息比如可执行文件中第几条机器指令对应源代码的第几行，但并不是把整个源文件嵌入到可执行文件中，所以在调试时必须保证gdb能找到源文件。

#### GDB命令

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

### 文件IO

#### 标准c库函数

`fopen fclose fread fwrite fgets fputs fscanf fprintf fseek fgetc fputc ftell feof fflush`

#### 文件指针

fopen返回值FILE* fp -> 数据结构FILE{文件描述符，文件读写指针，I/O缓冲区}

    文件描述符（整型值）：索引到对应的磁盘文件
    文件读写指针（位置）：读写文件过程中指针的实际位置
    I/O缓冲区（内存地址）：通过寻址找到对应的内存块

#### 虚拟地址空间

```
3G-4G: 内核区 Linux Kernel
    内存管理
    进程管理
    设备驱动管理
    VFS虚拟文件系统
0-3G:  用户区
    环境变量
    命令行参数
    栈空间
    共享库
    堆空间
    .bss未初始化全局变量
    .data已初始化全局变量
    .text二进制代码段
    受保护的地址0-4k
```

#### 文件描述符

PCB进程控制块 - 文件描述符表，数组默认大小1024

    0 -> STDIN_FILENO
    1 -> STDOUT_FILENO
    2 -> STDERR_FILENO
    -> /dev/tty当前终端

#### Linux系统IO函数

##### open & close

    int open(const char *pathname, int flags);
    int open(const char *pathname, int flags, mode_t mode); // mode & ~umask
    int creat(const char *pathname, mode_t mode);
    // flags:
    int close(int fd);

```c
// man 2 open
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
// man 3 perror
#include <stdio.h>
// man 2 close
#include <unistd.h>

int main() {
    // flags - int32, 每一位是一个标志位
    int fd = open("test.txt", O_RDWR | O_CREAT, 0777);

    if (fd == -1) {
        perror("open");
        return -1;
    }

    close(fd);

    return 0;
}
```

##### read & write

    ssize_t read(int fd, void *buf, size_t count);
    ssize_t write(int fd, const void *buf, size_t count);
    
```c
// man 2 read/write
#include <unistd.h>

int main() {
    int srcfd = open("source.txt", O_RDONLY);
    int dstfd = open("destination.txt", O_WRONLY | O_CREAT, 0777);

    if (srcfd == -1 || dstfd == -1) {
        perror("open");
        return -1;
    }

    char buf[1024];
    while ((len = read(srcfd, buf, sizeof(buf))) > 0) {
        write(dstfd, buf, len);
    }

    close(srcfd);
    close(dstfd);

    return 0;
}
```

##### lseek & fseek

    off_t lseek(int fd, off_t offset, int whence);
    int fseek(FILE *stream, long offset, int whence);
    // whence: SEEK_SET, SEEK_CUR, SEEK_END

```c
// man 2 lseek
#include <sys/types.h>
#include <unistd.h>
// man 3 fseek
#include <stdio.h>

int main() {
    int fd = open("test.txt", O_RDWR | O_CREAT, 0777);

    if (fd == -1) {
        perror("open");
        return -1;
    }

    int ret = lseek(fd, 100, SEEK_END);

    if (ret == -1) {
        perror("lseek");
        return -1;
    }

    close(fd);

    return 0;
}
```

##### stat & lstat

    int stat(const char *pathname, struct stat *statbuf);
    int lstat(const char *pathname, struct stat *statbuf);
    int fstat(int fd, struct stat *statbuf);
    
    struct stat {
        dev_t     st_dev;         /* ID of device containing file */
        ino_t     st_ino;         /* Inode number */
        mode_t    st_mode;        /* File type and mode */
        nlink_t   st_nlink;       /* Number of hard links */
        uid_t     st_uid;         /* User ID of owner */
        gid_t     st_gid;         /* Group ID of owner */
        dev_t     st_rdev;        /* Device ID (if special file) */
        off_t     st_size;        /* Total size, in bytes */
        blksize_t st_blksize;     /* Block size for filesystem I/O */
        blkcnt_t  st_blocks;      /* Number of 512B blocks allocated */

        struct timespec st_atim;  /* Time of last access */
        struct timespec st_mtim;  /* Time of last modification */
        struct timespec st_ctim;  /* Time of last status change */

    #define st_atime st_atim.tv_sec      /* Backward compatibility */
    #define st_mtime st_mtim.tv_sec
    #define st_ctime st_ctim.tv_sec
    };

    st_mode - 16位变量
        文件变量15-12
        /* Encoding of the file mode.  */
        #define	__S_IFMT	0170000	/* These bits determine file type.  */
        /* File types.  */
        #define	__S_IFDIR	0040000	/* Directory.  */
        #define	__S_IFCHR	0020000	/* Character device.  */
        #define	__S_IFBLK	0060000	/* Block device.  */
        #define	__S_IFREG	0100000	/* Regular file.  */
        #define	__S_IFIFO	0010000	/* FIFO.  */
        #define	__S_IFLNK	0120000	/* Symbolic link.  */
        #define	__S_IFSOCK	0140000	/* Socket.  */

        特殊权限位11-9
        /* Protection bits.  */
            #define	__S_ISUID	04000	/* Set user ID on execution.  */
            #define	__S_ISGID	02000	/* Set group ID on execution.  */
            #define	__S_ISVTX	01000	/* Save swapped text after use (sticky).  */
            11 - setGID 设置组id
            10 - setUID 设置用户组id
            09 - Sticky 粘住位
        8-0 ugorwx

```c
// man 2 stat
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
// man 3 getpwuid/getgrgid
#include <pwd.h>
#include <grp.h>

#include <stdint.h>
#include <time.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/sysmacros.h>

int main(int argc, char *argv[])
{
    struct stat sb;

    if (argc != 2) {
        fprintf(stderr, "Usage: %s <pathname>\n", argv[0]);
        exit(EXIT_FAILURE);
    }

    if (lstat(argv[1], &sb) == -1) {
        perror("lstat");
        exit(EXIT_FAILURE);
    }
    printf("ID of containing device:  [%jx,%jx]\n",
            (uintmax_t) major(sb.st_dev),
            (uintmax_t) minor(sb.st_dev));

    printf("File type:                ");

    switch (sb.st_mode & S_IFMT) {
    case S_IFBLK:  printf("block device\n");            break;
    case S_IFCHR:  printf("character device\n");        break;
    case S_IFDIR:  printf("directory\n");               break;
    case S_IFIFO:  printf("FIFO/pipe\n");               break;
    case S_IFLNK:  printf("symlink\n");                 break;
    case S_IFREG:  printf("regular file\n");            break;
    case S_IFSOCK: printf("socket\n");                  break;
    default:       printf("unknown?\n");                break;
    }
    printf("I-node number:            %ju\n", (uintmax_t) sb.st_ino);

    printf("Mode:                     %jo (octal)\n",
            (uintmax_t) sb.st_mode);

    printf("Link count:               %ju\n", (uintmax_t) sb.st_nlink);
    printf("Ownership:                UID=%ju   GID=%ju\n",
            (uintmax_t) sb.st_uid, (uintmax_t) sb.st_gid);

    printf("Preferred I/O block size: %jd bytes\n",
            (intmax_t) sb.st_blksize);
    printf("File size:                %jd bytes\n",
            (intmax_t) sb.st_size);
    // or lseek(fd, 0, SEEK_END)
    printf("Blocks allocated:         %jd\n",
            (intmax_t) sb.st_blocks);
    printf("Last status change:       %s", ctime(&sb.st_ctime));
    printf("Last file access:         %s", ctime(&sb.st_atime));
    printf("Last file modification:   %s", ctime(&sb.st_mtime));

    exit(EXIT_SUCCESS);
}
```

#### 文件属性操作函数

    int access(const char *pathname, int mode);
    // mode: R_OK, W_OK, X_OK, F_OK
    int chmod(const char *pathname, mode_t mode);
    int fchmod(int fd, mode_t mode);
    int chown(const char *pathname, uid_t owner, gid_t group);
    int lchown(const char *pathname, uid_t owner, gid_t group);
    int fchown(int fd, uid_t owner, gid_t group);
    int truncate(const char *path, off_t length);
    int ftruncate(int fd, off_t length);

```c
// man 2 access
#include <unistd.h>
// man 2 chmod
#include <sys/stat.h>
// man 2 chown
#include <unistd.h>
// man 2 truncate
#include <unistd.h>
#include <sys/types.h>

#include <stdio.h>
#include <stdlib.h>

int main() {
    if (access("test.txt", F_OK) == -1) {
        perror("access");
        exit(EXIT_FAILURE);
    }

    chmod("test.txt", 0777);

    return 0;
}
```

#### 目录操作函数

    int mkdir(const char *pathname, mode_t mode);
    int rmdir(const char *pathname);
    int rename(const char *oldpath, const char *newpath);
    int chdir(const char *path);
    int fchdir(int fd);
    char *getcwd(char *buf, size_t size);
    char *getwd(char *buf);
    char *get_current_dir_name(void);

```c
// man 2 mkdir
#include <sys/stat.h>
#include <sys/types.h>
// man 2 rmdir
#include <unistd.h>
// man 2 rename
#include <stdio.h>
// man 2 chdir
#include <unistd.h>
// man 2 getcwd
#include <unistd.h>
```

#### 目录遍历函数

    DIR *opendir(const char *name);
    DIR *fdopendir(int fd);
    struct dirent *readdir(DIR *dirp);
    int closedir(DIR *dirp);

```c
// man 3 opendir
#include <sys/types.h>
#include <dirent.h>
// man 3 readdir
#include <dirent.h>
// man 3 closedir
#include <sys/types.h>
#include <dirent.h>

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int getFileNum(const char* path) {
    DIR* dir = opendir(path);
    if (dir == NULL) {
        perror("opendir");
        exit(EXIT_FAILURE);
    }
    struct dirent* ptr;
    int ret = 0;
    while ((ptr = readdir(dir)) != NULL) {
        char* dname = ptr->d_name;
        if (strcmp(dname, ".") == 0 || strcmp(dname, "..") == 0) {
            continue;
        }
        if (ptr->d_type == DT_DIR) {
            char newpath[256];
            sprintf(newpath, "%s/%s", path, dname);
            ret += getFileNum(newpath);
        }
        else if (ptr->d_type == DT_REG) {
            ret += 1;
        }
    }
    closedir(dir);
    return ret;
}

int main(int argc, char* argv[]) {
    if (argc != 2) {
        fprintf(stderr, "Usage: %s path\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    int fn = getFileNum(argv[1]);
    printf("%d\n", fn);
    exit(EXIT_SUCCESS);
}


```

#### dup & dup2

    int dup(int oldfd);
    int dup2(int oldfd, int newfd);

```c
// man 2 dup
#include <unistd.h>

#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdio.h>
#include <string.h>

int main() {
    int fd1 = open("test1.txt", O_RDWR | O_CREAT, 0777);
    if (fd1 == -1) {
        perror("open");
        return -1;
    }
    int fd2 = dup(fd1);
    if (fd2 == -1) {
        perror("dup");
        return -1;
    }
    printf("old: %d, new: %d\n", fd1, fd2);
    int fd3 = open("test2.txt", O_RDWR | O_CREAT, 0777);
    int fd4 = dup2(fd1, fd3);

    char* str1 = (char*) "hello, ";
    char* str2 = (char*) "world!";
    write(fd1, str1, strlen(str1));
    write(fd2, str2, strlen(str2));
    write(fd3, str1, strlen(str1));
    write(fd4, str2, strlen(str2));
    close(fd1);
    close(fd2);
    close(fd3);
    close(fd4);
    return 0;
}
```

#### fcntl

    int fcntl(int fd, int cmd, ... /* arg */ );
    /*cmd
    Duplicating a file descriptor:  F_DUPFD, F_DUPFD_CLOEXEC, 
    File descriptor flags:          F_GETFD, F_SETFD, 
    File status flags:              F_GETFL, F_SETFL, 
    // File access mode (O_RDONLY, O_WRONLY, O_RDWR) and file creation flags (i.e., O_CREAT, O_EXCL, O_NOCTTY, O_TRUNC) in arg are ignored. On Linux, this command can change only the O_APPEND, O_ASYNC, O_DIRECT, O_NOATIME, and O_NONBLOCK flags. It is not possible to change the O_DSYNC and O_SYNC flags.
    Advisory record locking:        F_SETLK, F_SETLKW, F_GETLK, 
    Open file description locks (non-POSIX):    F_OFD_SETLK, F_OFD_SETLKW, F_OFD_GETLK
    Managing signals:               F_GETOWN, F_SETOWN, F_GETOWN_EX, F_SETOWN_EX, F_GETSIG, F_SETSI
    ......
    */

```c
// man 2 fcntl
#include <unistd.h>
#include <fcntl.h>
// man 3 printf

#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <string.h>

int main() {
    int fd = open("test.txt", O_RDWR | O_CREAT, 0777);
    if (fd == -1) {
        perror("open");
        return -1;
    }
    int ret = fcntl(fd, F_DUPFD);

    int fl = fcntl(ret, F_GETFL);
    fcntl(ret, F_SETFL, fl | O_APPEND);

    close(fd);
    close(ret)
    return 0;
}
```

## 进程

程序是包含一系列信息的文件，这些信息描述了如何在运行时创建一个进程：
- 二进制格式标识：每个程序文件都包含用于描述可执行文件格式的元信息。内核利用此信息来解释文件中的其他信息。（ELF可执行连接格式）
- 机器语言指令：对程序算法进行编码。
- 程序入口地址：标识程序开始执行时的起始指令位置。
- 数据：程序文件包含的变干初始值和程序使用的字面量值（比如字符串）。
- 符号表及重定位表：描述程序中函数和变量的位置及名称。这些表格有多重用途．其中包括调试和运行时的符号解析（动态链接）。
- 共享库和动态链接信息：程序文件所包含的一些字段，列出了程序运行时需要使用的共享库．以及加载共享库的动态连接器的路径名。
- 其他信息：程序文件还包含许多其他信息，用以描述如何创建进程。

- 进程是正在运行的程序的实例。是一个具有一定独立功能的程序关于某个数据集合的一次运行活动。它是操作系统动态执行的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元。
- 可以用一个程序来创建多个进程，进程是由内核定义的抽象实体并为该实体分配用以执行程序的各项系统资源。从内核的角度看，进程由用户内存空间和一系列内核数据结构组成，其中用户内存空间包含了程序代码及代码所使用的变量，而内核数据结构则用于维护进程状态信息。记录在内核数据结构中的信息包括许多与进程相关的标识号(IDs)、虚拟内存表、打开文件的描述符表、信号传递及处理的有关信息、进程资源使用及限制、当前工作目录和大量的其他信息。

时间片···

并行&并发···

Processing Control Block进程控制块
在/usr/src/linux-headers-xxx/include/linux/sched.h查看struct task_struct
进程id
进程状态
进程切换时需要保存和恢复的一些cpu寄存器
描述虚拟地址空间的信息
描述控制终端的信息
当前工作目录cwd
umask掩码
文件描述符表
和信号相关的信息
用户id和组id
会话session和进程组
进程可以使用的资源上限 $ulimit -a

三态模型：就绪，运行，阻塞
五态模型：新建，就绪，运行，阻塞，终止
阻塞：出现等待事件，又称等待态或者睡眠态

ps aux/ajx
top
kill

pid_t getpid/getppid/getpgid
pid_t fork //man 2 fork
写时拷贝copy on write
成功：父进程返回子进程号，子进程返回0
失败：父进程返回-1，errno - EAGAIN：系统进程数上限；ENOMEM：系统内存不足

父子进程的区别：
fork返回值
pcb中的pid,ppid,信号集

GDB多进程调试
set follow-fork-mode parent|child
set detach-on-fork on|off
info inferiors
inferior id
detach inferiors id

exec函数族
作用：根据指定的文件名找到可执行文件，并用它来取代调用进程的内容，即在调用进程内部执行一个可执行文件
执行成功后不会返回，代码段、数据段和堆栈等都被取代；只有调用失败了才会返回-1，从原程序的调用点接着往下执行
```c
#include <unistd.h>

extern char **environ;

int execl(const char *pathname, const char *arg, .../* (char  *) NULL */);
int execlp(const char *file, const char *arg, .../* (char  *) NULL */);
int execle(const char *pathname, const char *arg, .../*, (char *) NULL, char *const envp[] */);
int execv(const char *pathname, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execvpe(const char *file, char *const argv[], char *const envp[]);
int execve(const char *pathname, char *const argv[], char *const envp[]);
/*
l(list)         参数地址列表，以空指针结尾
v(vector)       存有各参数地址的指针数组的地址
p(path)         按PATH环境变量指定的目录搜索可执行文件
e(environment)  存有环境变量字符串地址的指针数组的地址
*/
```

进程控制
进程退出
```c
//exit相比_exit会在退出前刷新I/O缓冲、关闭文件描述符
#include <stdlib.h>

void exit(int status);
void _Exit(int status);

#include <unistd.h>

void _exit(int status);
```

孤儿进程Orphan Process
由init接管
僵尸进程Zombie Process
PCB等待父进程回收

进程回收
进程退出时释放用户区资源，但保留内核区主要是PCB，包括进程号、退出状态、运行时间等
```c
//man 2 wait
//wait()会阻塞，一次回收一个子进程
//waitpid()可以设置不阻塞，还可以指定等待哪个子进程结束
#include <sys/types.h>
#include <sys/wait.h>

pid_t wait(int *wstatus);
pid_t waitpid(pid_t pid, int *wstatus, int options);

//man 2 fork
#include <sys/types.h>
#include <unistd.h>

pid_t fork(void);

int main() {
    pid_t pid = fork();
    if (pid > 0) {
        printf("parent\n");
        // int ret = wait(NULL);
        int st;
        // int ret = wait(&st);
        int ret = waitpid(0, &st, WNOHANG);
        print("%d\n", ret);
        if (ret == -1) break;
        else if (ret == 0) continue;
        else if (ret > 0) {
            if (WIFEXITED(st)) {
                cout << WEXITSTATUS(st);
            }
            else if (WIFSIGNALED(st)) {
                cout << WTERMSIG(st);
            }
        }
    } else if (pid == 0) {
        printf("child\n");
    }

    return 0;
}
/*
退出状态宏函数
WIFEXITED(status)非0，进程正常退出
WEXITSTATUS(status)如果上宏为真，获取进程退出的状态（exit的参数）
WIFSIGNALED(status)非0，进程异常终止
WTERMSIG(status)如果上宏为真，获取使进程终止的信号编号
WIFSTOPPED(status)非0，进程处于暂停状态
WSTOPSIG(status)如果上宏为真，获取使进程暂停的信号的编号
WIFCONTINUED(status)非0，进程暫停后已经继续运行
*/

/*
waitpid:
pid > 0 : 某个子进程的pid
pid = 0 : 回收当前进程组的任意子进程
pid = -1 : 回收任意的子进程，相当于wait()
pid < -1 : 回收指定进程组的组id的负数
options = 0 : 阻塞
options > 0 : 非阻塞，WNOHANG WUNTRACED WCONTINUED
return > 0 : 返回子进程的pid
return = 0 : options = WNOHANG，非阻塞，还有子进程
return < 0 : 错误或没有子进程
*/
```

### 进程间通信

Inter Process Communication 
进程间通信的目的：数据传输，通知事件，资源共享，进程控制    
方式：管道，信号，消息队列，共享内存，信号量；Socket    

#### 匿名管道

特点：
在内核内存中维护的缓冲区，存储能力有限
有文件的特质，可以读写，匿名管道没有文件实体，有名管道有文件实体
是字节流，不存在消息或消息边界的概念
顺序读写，单向，半双工，一次性读取，无法使用lseek来随机访问数据
匿名管道只能在具有公共祖先（有亲缘关系）的进程之间使用

数据结构：
循环队列，读写双指针

使用：
man 2 pipe
    #include <unistd.h>
    int pipe(int pipefd[2]);
在fork之前创建，pipefd[0]读，pipefd[1]写

查看管道缓冲大小：
命令：ulimit -a
函数：
    #include <unistd.h>
    long fpathconf(int fd, int name);
    long pathconf(const char *path, int name);

子进程可以使用dup2(fd[1], STDOUT_FILENO)重定向

管道的读写特点：
使用管道时，需要注意以下几种特殊的情况（假设都是阻塞I/O操作）
1.所有的指向管道写端的文件描述符都关闭了（管道写端引用计数为0），有进程从管道的读端
读数据，那么管道中剩余的数据被读取以后，再次read会返回0，就像读到文件末尾一样。

2.如果有指向管道写端的文件描述符没有关闭（管道的写端引用计数大于0），而持有管道写端的进程
也没有往管道中写数据，这个时候有进程从管道中读取数据，那么管道中剩余的数据被读取后，
再次read会阻塞，直到管道中有数据可以读了才读取数据并返回。

3.如果所有指向管道读端的文件描述符都关闭了（管道的读端引用计数为0），这个时候有进程
向管道中写数据，那么该进程会收到一个信号SIGPIPE, 通常会导致进程异常终止。

4.如果有指向管道读端的文件描述符没有关闭（管道的读端引用计数大于0），而持有管道读端的进程
也没有从管道中读数据，这时有进程向管道中写数据，那么在管道被写满的时候再次write会阻塞，
直到管道中有空位置才能再次写入数据并返回。

总结：
    读管道：
        管道中有数据，read返回实际读到的字节数。
        管道中无数据：
            写端被全部关闭，read返回0（相当于读到文件的末尾）
            写端没有完全关闭，read阻塞等待

    写管道：
        管道读端全部被关闭，进程异常终止（进程收到SIGPIPE信号）
        管道读端没有全部关闭：
            管道已满，write阻塞
            管道没有满，write将数据写入，并返回实际写入的字节数

#### 有名管道/FIFO

FIFO有文件实体，但内容存放在内存中；进程退出后，FIFO继续保存在文件系统中

使用：
命令：mkfifo name
函数：man 3 mkfifo
    #include <sys/types.h>
    #include <sys/stat.h>
    int mkfifo(const char *pathname, mode_t mode);

demo：
```cpp
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <string.h>

int main(int argc, char *argv[]) {
    int ret, fd;
    pid_t cpid;
    // int pipefd[2];
    char buf[1024];
    if ((ret = access("pipe", F_OK)) == -1) {
        printf("Pipe file does not exist, creating...\n");
        // if (pipe(pipefd) == -1) perror("pipe");
        if ((ret = mkfifo("pipe", 0664)) == -1) {
            perror("mkfifo");
            exit(0);
        }
    }
    cpid = fork();
    if (cpid == -1) {
        perror("fork");
        exit(0);
    }
    if (cpid == 0) {
        // close(pipefd[0]);
        // fd = pipefd[1];
        if ((fd = open("pipe", O_WRONLY)) == -1) {
            perror("open");
            exit(0);
        }
        for (int i = 0; i < 100; ++i) {
            sprintf(buf, "hello %d\n", i);
            printf("write data: %s\n", buf);
            write(fd, buf, strlen(buf));
            sleep(1);
        }
        close(fd);
    } else if (cpid > 0) {
        // close(pipefd[1]);
        // fd = pipefd[0];
        if ((fd = open("pipe", O_RDONLY)) == -1) { // O_RDONLY|O_NONBLOCK
            perror("open");
            exit(0);
        }
        memset(buf, 0, sizeof(buf));
        while (read(fd, buf, sizeof(buf) > 0)) {
            printf("read data: %s\n", buf);
            memset(buf, 0, sizeof(buf));
        }
        close(fd);
    }
    return 0;
}
```

有名管道的注意事项：
    1.一个为只读而打开一个管道的进程会阻塞，直到另外一个进程为只写打开管道
    2.一个为只写而打开一个管道的进程会阻塞，直到另外一个进程为只读打开管道

读管道：
    管道中有数据，read返回实际读到的字节数
    管道中无数据：
        管道写端被全部关闭，read返回0，（相当于读到文件末尾）
        写端没有全部被关闭，read阻塞等待

写管道：
    管道读端被全部关闭，进行异常终止（收到一个SIGPIPE信号）
    管道读端没有全部关闭：
        管道已经满了，write会阻塞
        管道没有满，write将数据写入，并返回实际写入的字节数。

#### 内存映射（非阻塞）

```c
// man 2 mmap
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
int munmap(void *addr, size_t length);
/*
length: stat, lseek
prot: PROT_EXEC, PROT_READ, PROT_WRITE, PROT_NONE
flags: MAP_SHARED(sync), MAP_PRIVATE(copy), MAP_ANONYMOUS(匿名映射，fd=-1)
fd: open权限 >= prot
offset: 页大小的整数倍sysconf(_SC_PAGE_SIZE)
返回内存首地址
*/
```

#### 信号（软件中断）

```
$kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

|编号|信号名称|对应事件|默认动作|
|-|-|-|-|
|1|SIGHUP|用户退出 shell 时，由该 shell 启动的所有进程将收到这个信号|终止进程|
|**2**|**SIGINT**|当用户按下了 <Ctrl+c> 组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号|终止进程|
|**3**|**SIGQUIT**|用户按下 <Ctrl+\> 组合键时产生该信号，用户终端向正在运行中的由该终端启动的程序发出些信号|终止进程|
|4|SIGILL|CPU检测到某进程执行了非法指令|终止进程并产生 core 文件|
|5|SIGTRAP|该信号由断点指令或其他 trap 指令产生|终止进程并产生 core 文件|
|6|SIGABRT|调用 abort 函数时产生该信号|终止进程并产生 core 文件|
|7|SIGBUS|非法访问内存地址，包括内存对齐出错|终止进程并产生 core 文件|
|8|SIGFPE|在发生致命的运算错误时发出。不仅包括浮点运算错误，还包括溢出及除数为 0 等所有的算法错误|终止进程并产生 core 文件|
|**9**|**SIGKILL**|无条件终止进程。该信号不能被忽略，处理和阻塞|终止进程，可以杀死任何进程|
|10|SIGUSE1|用户定义的信号。即程序员可以在程序中定义并使用该信号|终止进程|
|**11**|**SIGSEGV**|指示进程进行了无效内存访问（段错误）|终止进程并产生 core 文件|
|12|SIGUSR2|另外一个用户自定义信号，程序员可以在程序中定义并使用该信号|终止进程|
|**13**|**SIGPIPE**|Broken pipe向一个没有读端的管道写数据|终止进程|
|14|SIGALRM|定时器超时，超时的时间由系统调用 alarm 设置|终止进程|
|15|SIGTERM|程序结束信号，与 SIGKILL 不同的是，该信号可以被阻塞和终止。通常用来要示程序正常退出。执行 shell 命令 kill 时，缺省产生这个信号|终止进程|
|16|SIGSTKFLT|Linux早期版本出现的信号，现仍保留向后兼容|终止进程|
|**17**|**SIGCHLD**|子进程结束时，父进程会收到这个信号|忽略这个信号|
|**18**|**SIGCONT**|如果进程已停止，则使其继续运行|继续/忽略|
|**19**|**SIGSTOP**|停止进程的执行。信号不能被忽略，处理和阻塞|为终止进程|
|20|SIGTSTP|停止终端交互进程的运行。按下 <Ctrl+z> 组合键时发出这个信号|暂停进程|
|21|SIGTTIN|后台进程读终端控制台|暂停进程|
|22|SIGTTOU|该信号类似于 SIGTTIN ，在后台进程要向终端输出数据时发生|暂停进程|
|23|SIGURG|套接字上有紧急数据时，向当前正在运行的进程发出些信号，报告有紧急数据到达。如网络带外数据到达|忽略该信号|
|24|SIGXCPU|进程执行时间超过了分配给该进程的 CPU 时间，系统产生该信号并发送给该进程|终止进程|
|25|SIGXFSZ|超过文件的最大长度设置|终止进程|
|26|SIGVTALRM|虚拟时钟超时时产生该信号。类似于 SIGALRM ，但是该信号只计算该进程占用 CPU 的使用时间|终止进程|
|27|SGIPROF|类似于 SIGVTALRM ，它不公包括该进程占用 CPU 时间还包括执行系统调用时间|终止进程|
|28|SIGWINCH|窗口变化大小时发出|忽略该信号|
|29|SIGIO|此信号向进程指示发出了一个异步 IO 事件|忽略该信号|
|30|SIGPWR|关机|终止进程|
|31|SIGSYS|无效的系统调用|终止进程并产生 core 文件|
|34~64|SIGRTMIN~SIGRTMAX|LINUX的实时信号，它们没有固定的含义（可以由用户自定义）|终止进程|

man 7 signal    

    disposition:
    Term   Default action is to terminate the process.
    Ign    Default action is to ignore the signal.
    Core   Default action is to terminate the process and dump core (see core(5)).
    Stop   Default action is to stop the process.
    Cont   Default action is to continue the process if it is currently stopped.

信号状态：产生、未决、递达  
SIGKILL 和 SIGSTOP 信号不能被捕捉、阻塞或者忽略，只能执行默认动作。 

信号相关的函数
```c
// man 2 kill
#include <sys/types.h>
#include <signal.h>
int kill(pid_t pid, int sig);
/*
pid > 0 : 将信号发送给指定的进程
pid = 0 : 将信号发送给当前的进程组
pid = -1 : 将信号发送给每一个有权限接收这个信号的进程
pid < -1 : 这个pid=某个进程组的ID取反 （-12345）
对照waitpid中的设定
sig = 0 : 不发送信号，但会执行存在性和权限检查
kill(getppid(), 9);
kill(getpid(), 9);
*/
// man 3 raise
#include <signal.h>
int raise(int sig); // == kill(getpid(), sig);
// man 3 abort
#include <stdlib.h>
void abort(void); // == kill(getpid(), SIGABRT);
// man 2 alarm
#include <unistd.h>
unsigned int alarm(unsigned int seconds); // 发送SIGALRM信号，非阻塞
/*
seconds = 0时，定时器无效，alarm(0)用于取消一个定时器
返回之前的定时器剩余的时间
与进程状态无关（自然定时法）：无论进程处于什么状态，alarm都会继续计时
*/
// man 2 setitimer
#include <sys/time.h>
int getitimer(int which, struct itimerval *curr_value);
int setitimer(int which, const struct itimerval *new_value, struct itimerval *old_value);
struct itimerval {
    struct timeval it_interval; /* Interval for periodic timer */
    struct timeval it_value;    /* Time until next expiration */
};
struct timeval {
    time_t      tv_sec;         /* seconds */
    suseconds_t tv_usec;        /* microseconds */
};
/*
周期定时器：精度更高
which: 定时器以什么时间类型计时
    ITIMER_REAL: 真实世界时间，SIGALRM
    ITIMER_VIRTUAL: 进程所耗的用户模式CPU时间，SIGVTALRM
    ITIMER_PROF: 进程所耗的用户和内核模式总CPU时间，SIGPROF
new_value: 设定定时
old_value: 返回上个定时器的定时，常置为NULL
*/
// man 2 signal - ANSI C signal handling
#include <signal.h>
typedef void (*sighandler_t)(int);
sighandler_t signal(int signum, sighandler_t handler);
/*
disposition: SIG_IGN, SIG_DFL, signal_handler
The signals SIGKILL and SIGSTOP cannot be caught or ignored.
成功返回上次注册的信号处理函数的地址，首次返回NULL
失败返回SIG_ERR
*/
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```

#### 信号集

许多信号相关的系统调用都需要能表示一组不同的信号，多个信号可使用一个称之为信号集的数据结构来表示，其系统数据类型为 sigset_t 
在 PCB 中有两个非常重要的信号集。一个称之为“阻塞信号集”，另一个称之为“未决信号集”。这两个信号集都是内核使用位图机制来实现的。但操作系统不允许我们直接对这两个信号集进行位操作。而需自定义另外一个集合，借助信号集操作函数来对 PCB 中的这两个信号集进行修改    
信号的 “未决” 是一种状态，指的是从信号的产生到信号被处理前的这一段时间。
信号的 “阻塞” 是一个开关动作，指的是阻止信号被处理，但不是阻止信号产生。
信号的阻塞就是让系统暂时保留信号留待以后发送。由于另外有办法让系统忽略信号，所以一般情况下信号的阻塞只是暂时的，只是为了防止信号打断敏感的操作。

阻塞信号集阻塞未决信号集中的信号

信号集相关的函数
```c
// 修改自定义信号集
// man 3 sigsetops
#include <signal.h>
int sigemptyset(sigset_t *set);
int sigfillset(sigset_t *set);
int sigaddset(sigset_t *set, int signum); 
int sigdelset(sigset_t *set, int signum);
int sigismember(const sigset_t *set, int signum);
/*
sigemptyset() initializes the signal set given by set to empty, with all signals excluded from the set.
sigfillset() initializes set to full, including all signals.
sigaddset() and sigdelset() add and delete respectively signal signum from set.
sigismember() tests whether signum is a member of set.
*/
// 修改阻塞信号集
// man 2 sigprocmask
#include <signal.h>
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
/*
how: 
    SIG_BLOCK: mask |= set
    SIG_UNBLOCK: mask &= ~set
    SIG_SETMASK: mask = set
获取：sigprocmask(~, NULL, ptr)
*/
// 获取未决信号集
// man 2 sigpending
#include <signal.h>
int sigpending(sigset_t *set);
``` 
```c
// man 2 sigaction
// avoid to use signal
#include <signal.h>
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
struct sigaction {
    void     (*sa_handler)(int); // 信号处理函数
    void     (*sa_sigaction)(int, siginfo_t *, void *);
    sigset_t   sa_mask; // 临时阻塞信号集
    int        sa_flags; // 0使用sa_handler，SA_SIGINFO使用sa_sigaction，其他...
    void     (*sa_restorer)(void); // 不用
};
```

内核实现信号捕捉的过程
1. 在执行主控制流程的某条指令时因为中断、异常或系统调用进入内核
2. 内核处理完异常准备回用户模式之前先处理当前进程中可以递送的信号 do_signal()
3. 如果信号的处理动作为自定义的信号处理函数，则回到用户模式执行信号处理函数（而不是回到主控制流程）void sighandler (int)
4. 信号处理函数返回时执行特殊的系统调用 sigreturn 再次进入内核 sys_sigreturn()
5. 返回用户模式从主控制流程中上次被中断的地方继续向下执行

SIGCHLD 信号产生的条件
子进程终止时
子进程接收到 SIGSTOP 信号停止时
子进程处在停止态，接受到 SIGCONT 后唤醒时
以上三种条件都会给父进程发送 SIGCHLD 信号，父进程默认会忽略该信号

信号处理函数中使用不可重入函数如printf等可能会导致出现意想不到的段错误

#### 共享内存（无需内核介入）

使用步骤：
1. 调用 shmget() 创建一个新共享内存段或取得一个既有共享内存段的标识符（即由其他进程创建的共享内存段）。这个调用将返回后续调用中需要用到的共享内存标识符。
2. 使用 shmat() 来附上共享内存段，即使该段成为调用进程的虚拟内存的一部分。
3. 此刻在程序中可以像对待其他可用内存那样对待这个共享内存段。为引用这块共享内存，
4. 程序需要使用由 shmat() 调用返回的 addr 值，它是一个指向进程的虚拟地址空间中该共享内存段的起点的指针。
5. 调用 shmdt() 来分离共享内存段。在这个调用之后，进程就无法再引用这块共享内存了。这一步是可选的，并且在进程终止时会自动完成这一步。
6. 调用 shmctl() 来删除共享内存段。只有当当前所有附加内存段的进程都与之分离之后内存段才会销毁。只有一个进程需要执行这一步。

函数：
```c
// man 2 shmget
#include <sys/ipc.h>
#include <sys/shm.h>
int shmget(key_t key, size_t size, int shmflg);
/*
- 功能：创建一个新的共享内存段，或者获取一个既有的共享内存段的标识。
    新创建的内存段中的数据都会被初始化为0
- 参数：
    - key : key_t类型是一个整形，通过这个找到或者创建一个共享内存。
            一般使用16进制表示，非0值
    - size: 共享内存的大小，round up to page size
    - shmflg: 属性
        - 访问权限
        - 附加属性：创建/判断共享内存是不是存在
            - 创建：IPC_CREAT
            - 判断共享内存是否存在： IPC_EXCL , 需要和IPC_CREAT一起使用
                IPC_CREAT | IPC_EXCL | 0664
    - 返回值：
        失败：-1 并设置错误号
        成功：>0 返回共享内存的引用的ID，后面操作共享内存都是通过这个值。
*/
// man 2 shmop
#include <sys/types.h>
#include <sys/shm.h>
void *shmat(int shmid, const void *shmaddr, int shmflg);
/*
- 功能：和当前的进程进行关联
- 参数：
    - shmid : 共享内存的标识（ID）,由shmget返回值获取
    - shmaddr: 申请的共享内存的起始地址，指定NULL，内核指定
    - shmflg : 对共享内存的操作
        - 读 ： SHM_RDONLY, 必须要有读权限
        - 读写： 0
- 返回值：
    成功：返回共享内存的首（起始）地址。  失败(void *) -1
*/
int shmdt(const void *shmaddr);
/*
- 功能：解除当前进程和共享内存的关联
- 参数：
    shmaddr：共享内存的首地址
- 返回值：成功 0， 失败 -1
*/
// man 2 shmctl
#include <sys/ipc.h>
#include <sys/shm.h>
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
struct shmid_ds {
    struct ipc_perm shm_perm;    /* Ownership and permissions */
    size_t          shm_segsz;   /* Size of segment (bytes) */
    time_t          shm_atime;   /* Last attach time */
    time_t          shm_dtime;   /* Last detach time */
    time_t          shm_ctime;   /* Creation time/time of last
                                    modification via shmctl() */
    pid_t           shm_cpid;    /* PID of creator */
    pid_t           shm_lpid;    /* PID of last shmat(2)/shmdt(2) */
    shmatt_t        shm_nattch;  /* No. of current attaches */
    ...
};
/*
- 功能：对共享内存进行操作。删除共享内存，共享内存要删除才会消失，创建共享内存的进程被销毁了对共享内存是没有任何影响。
- 参数：
    - shmid: 共享内存的ID
    - cmd : 要做的操作
        - IPC_STAT : 获取共享内存的当前的状态
        - IPC_SET : 设置共享内存的状态
        - IPC_RMID: 标记共享内存被销毁
    - buf：需要设置或者获取的共享内存的属性信息
        - IPC_STAT : buf存储数据
        - IPC_SET : buf中需要初始化数据，设置到内核中
        - IPC_RMID : 没有用，NULL
*/
// man 3 ftok
#include <sys/types.h>
#include <sys/ipc.h>
key_t ftok(const char *pathname, int proj_id);
/*
- 功能：根据指定的路径名，和int值，生成一个IPC key。用于msgget, semget, shmget
- 参数：
    - pathname:指定一个存在的路径
        /home/nowcoder/Linux/a.txt
        / 
    - proj_id: int类型的值，但是这系统调用只会使用其中的1个字节
                范围 ： 0-255  一般指定一个字符 'a'
- 两个进程如在pathname和proj_id上达成一致（即约定好），双方就都能够通过调用ftok函数得到同一个IPC键。
- ftok调用返回的整数IPC键由proj_id的低序8位，st_dev成员的低序8位，st_info的低序16位组合而成。
*/
int main() {
    int shmid = shmget(100, 4096, IPC_CREAT|0664); // 创建
    // int shmid = shmget(100, 0, IPC_CREAT); // 获取
    void *ptr = shmat(shmid, NULL, 0);
    char *str = "hello, world";
    memcpy(ptr, str, strlen(str) + 1);
    shmdt(ptr);
    shmctl(shmid, IPC_RMID, NULL);
}
```

命令：
ipcs 用法
    ipcs -a // 打印当前系统中所有的进程间通信方式的信息
    ipcs -m // 打印出使用共享内存进行进程间通信的信息
    ipcs -q // 打印出使用消息队列进行进程间通信的信息
    ipcs -s // 打印出使用信号进行进程间通信的信息

ipcrm 用法
    ipcrm -M shmkey // 移除用 shmkey 创建的共享内存段
    ipcrm -m shmid // 移除用 shmid 标识的共享内存段
    ipcrm -Q msgkey // 移除用 msqkey 创建的消息队列
    ipcrm -q msqid // 移除用 msqid 标识的消息队列
    ipcrm -S semkey // 移除用 semkey 创建的信号
    ipcrm -s semid // 移除用 semid 标识的信号

共享内存和内存映射的区别
1. 共享内存可以直接创建，内存映射需要磁盘文件（匿名映射除外）
2. 共享内存效率更高(I/O)
3. 内存
    所有的进程操作的是同一块共享内存。
    内存映射，每个进程在自己的虚拟地址空间中有一个独立的内存。
4. 数据安全
    - 进程突然退出
        共享内存还存在
        内存映射区消失
    - 运行进程的电脑死机，宕机了
        数据存在在共享内存中，没有了
        内存映射区的数据 ，由于磁盘文件中的数据还在，所以内存映射区的数据还存在。
5. 生命周期
    - 内存映射区：进程退出，内存映射区销毁
    - 共享内存：进程退出，共享内存还在，标记删除（所有的关联的进程数为0），或者关机
        如果一个进程退出，会自动和共享内存进行取消关联。

#### 消息队列

在Linux中，IPC消息队列是一个双向通信的全内存设计，即内核保证了读写顺序和数据同步，并且是性能比较好的先进先出的数据结构。消息队列的应用场景：比如异步任务处理，抢占式的数据分发，顺序缓存区等。

消息队列的产生原因
消息队列其实就是消息传输过程中保存消息的容器，既然有了管道，为什么要出现消息队列呢？理由如下：

（1）生命周期：匿名管道和命名管道都是随进程的，意味着管道的生命周期是随进程的退出而退出的。

（2）传送方式：管道传送数据时以无格式字节流的形式传送，给程序的开发带来不便。

（3）信号传递量：担当数据传送媒介的管道，其缓冲区大小受到较大的限制。

鉴于上面的三种原因：IPC方式下的另一种进程间通信——>消息队列应运而生。它克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点。

什么是消息队列？
[普通定义]：消息队列提供了一种从一个进程向另一个进程发送一个数据块的方法，每个数据块都被认为含有一个类型，接受者接受的数据块可以有不同的类型值，我们可以通过发送消息来避免命名管道的同步和阻塞问题。

[最佳定义]：内核地址空间中的内部链表，消息可以顺序地发送到队列中，并以几种不同的方式从队列中获取，每个消息队列是由IPC标识符所唯一标识的。

消息队列与管道的不同在于：消息队列是基于消息的，而管道是基于字节流的，且消息队列的读取不一定是先入先出，而且如果你没有显示的删除它，那么在关机之前它一直存在。

消息队列的上限
消息队列与管道也是一样的不足，就是每个消息队列的最大长度是有上限的（MSGMAX），每个消息队列的总的字节数是有上限（MSGMNB），系统上消息队列的总数也是有一个上限（MSGMNI）。

IPC对象数据结构
内核为每个IPC对象维护了一个数据结构（存在于/usr/include/linux/ipc.h）
不仅如此，消息队列，共享内存和信号量都有这样一个共同的数据结构。

```c
struct ipc_perm
{
        __kernel_key_t  key;//端口号
        __kernel_uid_t  uid;//所有者的用户ID
        __kernel_gid_t  gid;//所有者组ID
        __kernel_uid_t  cuid;//创建者的用户ID
        __kernel_gid_t  cgid;//创建者的组ID
        __kernel_mode_t mode; //访问模式
        unsigned short  seq;//顺序值
};
```

消息队列结构
消息队列结构存在于/usr/include/linux/msg.h目录下：
```c
struct msqid_ds {
        struct ipc_perm msg_perm;    /* IPC对象结构体 */
        struct msg *msg_first;       /*消息队列头指针*/
        struct msg *msg_last;        /*消息队列尾指针*/
        __kernel_time_t msg_stime;   /*最后一次插入消息队列消息的时间*/
        __kernel_time_t msg_rtime;   /*最后一次接收消息即删除队列中一个消息的时间*/
        __kernel_time_t msg_ctime;   /* 最后修改队列的时间*/
        unsigned long  msg_lcbytes;  /* Reuse junk fields for 32 bit */
        unsigned long  msg_lqbytes;  /* ditto */
        unsigned short msg_cbytes;   /*队列上所有消息总的字节数 */
        unsigned short msg_qnum;     /*在当前队列上消息的个数 */
        unsigned short msg_qbytes;   /* 队列最大的字节数 */
        __kernel_ipc_pid_t msg_lspid;/* 发送最后一条消息的进程的pid */
        __kernel_ipc_pid_t msg_lrpid;/* 接收最后一条消息的进程的pid */
};
```
从上图可以看出，消息队列结构体中的第一条内容就是IPC结构体，即IPC结构体是共用的，后面的都是消息队列所有的成员，并且消息队列是由链表来实现的。

总结：

（1）消息队列可以独立于发送和接收数据的进程而存在，从而消除了在同步命名管道的打开和关闭可能产生的困难。

（2）可以通过发送消息可以避免命名管道的同步和阻塞问题，不需要由进程自己来提供同步方法。

（3）接受程序可以通过消息类型有选择的接收数据，而不像命名管道中的那样，只能默认的接受。

（4）双向通信，生命周期随内核。

使用步骤：
1. ftok()生产key
2. 使用msgget() 创建/获取消息队列，返回值是队列标识符
    ```c
    #include <sys/msg.h>
    int msgget(key_t key,int flag);
    /*
    返回值：成功返回一个消息队列的msgid，作为进程对它的唯一标识；失败返回-1。

    参数key便不再多说，就上我们上面提到的key值。flag为创建的方式（IPC_CREAT和IPC_EXCL），一般有以下两种选项：

    IPC_CREAT单独使用，表示申请创建一个IPC资源，若申请的资源已经存在，则直接获取；若不存在，则创建新的IPC资源。
    IPC_CREAT与IPC_EXCL一起使用，表示申请创建一个IPC资源，若申请的资源不存在，则创建新的IPC资源；若已经存在，则返回-1。
    IPC_EXCL单独使用没有任何意义，他的存在就是与IPC_CREAT一起使用，从而保证得到的是一个新创建的资源。
    */
    ```
3. 使用msgsnd() 发送消息
    ```c
    #include<sys/msg.h>
    int msgsnd(int msqid,const void* ptr,size_t nbytes,int flag)
    /*
    返回值：成功返回0，失败返回-1。
    我们一般让ptr指针指向一个结构体，结构如下：
    */
    struct msgbuf
    {
        long mtype;
        char mtext[MYSIZE];
    };
    /*
    将mtype设置为要发送数据的数据类型，mtext存放发送的数据。这也是为什么我们在上面说，消息队列传输的是有类型的数据。

    nbytes为传输数据的大小。
    flag可设置称IPC_NOWAIT，也可以不设置（设为0）。IPC_NOWAIT类似于文件I/O的非阻塞I/O标志。当消息队列已满，指定IPC_NOWAIT后，msgsnd立即出错返回EAGAIN，如果没有指定，该进程将会一直阻塞。知道直到消息队列有空间容纳发送的消息或者系统删除了此消息队列或者收到哦了一个信号，从信号处理程序中返回。
    */
    ```
    使用msgrcv() 接收消息
    ```c
    #include<sys/msg.h>
    ssize_t msgrcv(int msqid, void *msgp, size_t msgsz, long msgtyp,int msgflg);
    /*
    返回值：成功返回接受到的消息的bytes，失败返回-1。
    第二个参数msgp为存放接受到消息的缓冲区，msgsz为指定缓冲区的大小。type为我们接受到数据的类型。
    如果接受到的消息大于缓冲区的大小，并且msgflg设置了MSG_NOERROR的话，消息就会发生截断并丢弃被截去的部分。也可以将msgflag设置成IPC_NOWAIT，使操作不阻塞。
    */
    ```
4. 使用msgctl() 删除消息队列
    ```c
    #include <sys/msg.h>
    int msgctl(int msgid, int command, struct msgid_ds *buf);
    /*
    command是将要采取的动作，它可以取3个值，
        IPC_STAT：把msgid_ds结构中的数据设置为消息队列的当前关联值，即用消息队列的当前关联值覆盖msgid_ds的值。
        IPC_SET：如果进程有足够的权限，就把消息列队的当前关联值设置为msgid_ds结构中给出的值
        IPC_RMID：删除消息队列
    buf是指向msgid_ds结构的指针，它指向消息队列模式和访问权限的结构。msgid_ds结构至少包括以下成员：
    */
    struct msgid_ds {
        uid_t shm_perm.uid;
        uid_t shm_perm.gid;
        mode_t shm_perm.mode;
    };
    ```

##### demo

```c
// sendmsg.c

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#include<string.h>

struct my_msg
{
    int mtype; // 消息类型
    char buf[256];
}msg1, msg2;

int main()
{
    key_t key = ftok("./", 88);

    int msgid = msgget(key, 0666|IPC_CREAT);
    if(-1 == msgid)
    {
        perror("msgget failed!!!");
        exit(1);
    }

    msg1.mtype = 2;
    strcpy(msg1.buf, "hello, msg2");
    msgsnd(msgid, &msg1, sizeof(msg1), 0); // 阻塞
//    msgsnd(msgid, &msg1, sizeof(msg1), IPC_NOWAIT); // 非阻塞

    msg2.mtype = 1;
    strcpy(msg2.buf, "hello, msg1");
    msgsnd(msgid, &msg2, sizeof(msg2), 0); // 阻塞

    printf("消息发送完成，按回车销毁消息队列\n");
    getchar();

    if(-1 == shmctl(msgid, IPC_RMID, NULL))
    {
        perror("shmctl failed");
        exit(2);
    }
    return 0;
}

// recvmsg.c

#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#include<string.h>

struct my_msg
{
    int mtype; // 消息类型
    char buf[256];
}msg;

int main()
{
    key_t key = ftok("./", 88);

    // 获取消息队列
    int msgid = msgget(key, 0);
    if(-1 == msgid)
    {
        perror("msgget failed!!!");
        exit(1);
    }

    int res = msgrcv(msgid, &msg, sizeof(msg),
            2, // 取消息类型为2的消息
            0);
    printf("类型：%d, 内容：%s\n", msg.mtype, msg.buf);

    printf("消息接收完成，按回车销毁消息队列\n");
    getchar();

    if(-1 == shmctl(msgid, IPC_RMID, NULL))
    {
        perror("shmctl failed");
        exit(2);
    }
    return 0;
}
```


#### 守护进程

终端
在 UNIX 系统中，用户通过终端登录系统后得到一个 shell 进程，这个终端成为 shell 进程的控制终端（ Controlling Terminal ），进程中，控制终端是保存在 PCB 中的信息，而 fork() 会复制 PCB 中的信息，因此由 shell 进程启动的其它进程的控制终端也是这个终端。
默认情况下（没有重定向），每个进程的标准输入、标准输出和标准错误输出都指向控制终端，进程从标准输入读也就是读用户的键盘输入，进程往标准输出或标准错误输出写也就是输出到显示器上。
在控制终端输入一些特殊的控制键可以给前台进程发信号，例如 Ctrl + C 会产生 SIGINT 信号， Ctrl + \ 会产生 SIGQUIT 信号。

进程组
进程组和会话在进程之间形成了一种两级层次关系：进程组是一组相关进程的集合，会话是一组相关进程组的集合。进程组和会话是为支持 shell 作业控制而定义的抽象概念，用户通过 shell 能够交互式地在前台或后台运行命令。
进行组由一个或多个共享同一进程组标识符（ PGID ）的进程组成。一个进程组拥有一个进程组首进程，该进程是创建该组的进程，其进程 ID 为该进程组的 ID ，新进程会继承其父进程所属的进程组 ID 。
进程组拥有一个生命周期，其开始时间为首进程创建组的时刻，结束时间为最后一个成员进程退出组的时刻。一个进程可能会因为终止而退出进程组，也可能会因为加入了另外一个进程组而退出进程组。进程组首进程无需是最后一个离开进程组的成员。

会话
会话是一组进程组的集合。会话首进程是创建该新会话的进程，其进程 ID 会成为会话 ID 。新进程会继承其父进程的会话 ID 。
一个会话中的所有进程共享单个控制终端。控制终端会在会话首进程首次打开一个终端设备时被建立。一个终端最多可能会成为一个会话的控制终端。
在任一时刻，会话中的其中一个进程组会成为终端的前台进程组，其他进程组会成为后台进程组。只有前台进程组中的进程才能从控制终端中读取输入。当用户在控制终端中输入终端字符生成信号后，该信号会被发送到前台进程组中的所有成员。
当控制终端的连接建立起来之后，会话首进程会成为该终端的控制进程。

函数
```c
// man 2 setpgid
#include <sys/types.h>
#include <unistd.h>

int setpgid(pid_t pid, pid_t pgid);
pid_t getpgid(pid_t pid);

pid_t getpgrp(void);                 /* POSIX.1 version */
pid_t getpgrp(pid_t pid);            /* BSD version */

int setpgrp(void);                   /* System V version */
int setpgrp(pid_t pid, pid_t pgid);  /* BSD version */

// man 2 getsid/setsid
#include <sys/types.h>
#include <unistd.h>

pid_t getsid(pid_t pid);
pid_t setsid(void);
```

守护进程
守护进程（ Daemon Process ），也就是通常说的 Daemon 进程（精灵进程），是Linux 中的后台服务进程。它是一个生存期较长的进程，通常独立于控制终端并且周期性地执行某种任务或等待处理某些发生的事件。一般采用以 d 结尾的名字。
守护进程具备下列特征：
生命周期很长，守护进程会在系统启动的时候被创建并一直运行直至系统被关闭。
它在后台运行并且不拥有控制终端。没有控制终端确保了内核永远不会为守护进程自动生成任何控制信号以及终端相关的信号（如 SIGINT 、 SIGQUIT ）。
Linux 的大多数服务器就是用守护进程实现的。比如， Internet 服务器 inetd，Web 服务器 httpd 等。

守护进程创建步骤
1. 执行一个 fork()，之后父进程退出，子进程继续执行。
2. 子进程调用 setsid() 开启一个新会话。
3. 清除进程的 umask 以确保当守护进程创建文件和目录时拥有所需的权限。
4. 修改进程的当前工作目录，通常会改为根目录（/）。
5. 关闭守护进程从其父进程继承而来的所有打开着的文件描述符。
6. 在关闭了文件描述符 0、1、2 之后，守护进程通常会打开 /dev/null 并使用 dup2()使所有这些描述符指向这个设备。
7. 核心业务逻辑

```c
#include <stdio.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/time.h>
#include <signal.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>

void work(int num) {
    // man 2 time
    time_t tm = time(NULL);
    // man 3 ctime
    struct tm * loc = localtime(&tm);
    // char buf[1024];
    // sprintf(buf, "%d-%d-%d %d:%d:%d\n",loc->tm_year,loc->tm_mon
    // ,loc->tm_mday, loc->tm_hour, loc->tm_min, loc->tm_sec);
    // printf("%s\n", buf);
    char * str = asctime(loc);
    int fd = open("time.txt", O_RDWR | O_CREAT | O_APPEND, 0664);
    write(fd ,str, strlen(str));
    close(fd);
}

int main() {
    pid_t pid = fork();
    if(pid > 0) exit(0);
    setsid();
    umask(022);
    chdir("/");
    int fd = open("/dev/null", O_RDWR);
    dup2(fd, STDIN_FILENO);
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);
    // 信号捕捉
    struct sigaction act;
    act.sa_flags = 0;
    act.sa_handler = work;
    sigemptyset(&act.sa_mask);
    sigaction(SIGALRM, &act, NULL);
    // 周期定时器
    struct itimerval val;
    val.it_value.tv_sec = 2;
    val.it_value.tv_usec = 0;
    val.it_interval.tv_sec = 2;
    val.it_interval.tv_usec = 0;
    setitimer(ITIMER_REAL, &val, NULL);
    while(1) sleep(10);
    return 0;
}
```

## 线程

与进程（ process ）类似，线程 thread ）是允许应用程序并发执行多个任务的一种机制。一个进程可以包含多个线程。同一个程序中的所有线程均会独立执行相同程序，且共享同一份全局内存区域，其中包括初始化数据段、未初始化数据段，以及堆内存段。（传统意义上的 UNIX 进程只是多线程程序的一个特例，该进程只包含一个线程）
进程是 CPU 分配资源的最小单位，线程是操作系统调度执行的最小单位。
线程是轻量级的进程（ LWP: Light Weight Process ），在 Linux 环境下线程的本质仍是进程。
查看指定进程的 LWP 号： ps -Lf pid

线程和进程区别
进程间的信息难以共享。由于除去只读代码段外，父子进程并未共享内存，因此必须采用一些进程间通信方式，在进程间进行信息交换。
调用 fork() 来创建进程的代价相对较高，即便利用写时复制技术，仍然需要复制诸如内存页表和文件描述符表之类的多种进程属性，这意味着 fork() 调用在时间上的开销依然不菲。
线程之间能够方便、快速地共享信息。只需将数据复制到共享（全局或堆）变量中即可。
创建线程比创建进程通常要快 10 倍甚至更多。线程间是共享虚拟地址空间的，无需采用写时复制来复制内存，也无需复制页表。

共享资源
进程 ID 和父进程 ID
进程组 ID 和会话 ID
用户 ID 和 用户组 ID
文件描述符表
信号处置
文件系统的相关信息：文件权限掩码（umask）、当前工作目录
虚拟地址空间（除栈、 .text）

非共享资源
线程 ID
信号掩码
线程特有数据
error 变量
实时调度策略和优先级
栈，本地变量和函数的调用链接信息

NPTL

当 Linux 最初开发时，在内核中并不能真正支持线程。但是它的确可以通过 clone()
系统调用将进程作为可调度的实体。这个调用创建了调用进程（ calling process ）的
一个拷贝，这个拷贝与调用进程共享相同的地址空间。 LinuxThreads 项目使用这个调用
来完成在用户空间模拟对线程的支持。不幸的是，这种方法有一些缺点，尤其是在信号处
理、调度和进程间同步等方面都存在问题。另外，这个线程模型也不符合 POSIX 的要求。

要改进 LinuxThreads ，需要内核的支持，并且重写线程库。有两个相互竞争的项目开始
来满足这些要求。一个包括 IBM 的开发人员的团队开展了 NGPT Next Generation
POSIX Threads ）项目。同时 Red Hat 的一些开发人员开展了 NPTL 项目。 NGPT
在 2003 年中期被放弃了，把这个领域完全留给了 NPTL 。

NPTL ，或称为 Native POSIX Thread Library ，是 Linux 线程的一个新实现，它
克服了 LinuxThreads 的缺点，同时也符合 POSIX 的需求。与 LinuxThreads 相
比，它在性能和稳定性方面都提供了重大的改进。

查看当前 pthread 库版本： getconf GNU_LIBPTHREAD_VERSION


线程操作

int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
void *(*start_routine) (void *), void *arg);

pthread_t pthread_self(void);

int pthread_equal(pthread_t t1, pthread_t t2);

void pthread_exit(void *retval);

int pthread_join(pthread_t thread, void **retval);

int pthread_detach(pthread_t thread);

int pthread_cancel(pthread_t thread);


线程属性

线程属性类型 pthread_attr_t

int pthread_attr_init(pthread_attr_t *attr);

int pthread_attr_destroy(pthread_attr_t *attr);

int pthread_attr_getdetachstate(const pthread_attr_t *attr, int
*

int pthread_attr_setdetachstate(pthread_attr_t *attr, int
detachstate);