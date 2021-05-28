## Linux编程基础

### 1. Linux简介

- UNIX系统分叉：

  <img src="/Users/sissiliu/Desktop/figure_1_3.png" alt="figure_1_3"  />

*ps -aux (BSD) & ps -ef (System V)?*

- Linux发行版谱系：

  ![figure_1_5](/Users/sissiliu/Desktop/figure_1_5.jpeg)

- 文件与目录管理：
  - 绝对路径：路径的写法，由根目录`/`写起，例如：`/usr/share/doc`
  - 路径的写法，不是由`/`写起，例如：由目录`/usr/share/doc`到`/usr/share/man`目录，可以写成`cd ../man`这就是相对路径的写法。

- Some useful Linux command:
  - 文件
    - 快速查找某个文件：`# whereis [filename]` 或者 `# find [目录] -name [filename]`
    - 查看文件类型：`# file [filename]`
    - 显示文件倒数6行内容：`# tail -n 6 [filename]`
    - 让tail不停地读最新的内容：`# tail -n 10 -f [filename]`
    - 查找包含xxx字符串的文件：`# grep -l -r [xxx]`
    - 查看文件中间的第五行（含）到第10行（含）的内容 ：`# sed -n ‘5,10p’ [filename]`
    - <u>查找关于xxx的命令</u>：`# apropos [xxx] man -k [xxx]`
    - 把所有文件名中的大写改为小写：`# rename ‘tr/A-Z/a-z/’` 
    - <u>把所有文件的后辍由rm改为rmvb</u>：`# rename ’s/.rm$/.rmvb/’`
    - 删除特殊文件名的文件，如文件名：`–help.txt: # rm - –help.txt` 或者 `rm ./–help.txt`
  - （解）压缩
    - 解压缩 xxx.tar.gz：`#tar -zxvf xxx.tar.gz`
    - 解压缩 xxx.tar.bz2：`#tar -jxvf xxx.tar.bz2`
    - 压缩aaa bbb目录为xxx.tar.gz：`#tar -zcvf xxx.tar.gz aaa bbb`
    - 压缩aaa bbb目录为xxx.tar.bz2：`#tar -jcvf xxx.tar.bz2 aaa bbb`
    - 解压缩 RAR 文件 
      - 先安装：`#sudo apt-get install rar unrar #sudo ln -f /usr/bin/rar /usr/bin/unrar`
      - 解压：`#unrar x aaaa.rar`
    - 解压缩 ZIP 文件 
      - 先安装：`#sudo apt-get install zip unzip #sudo ln -f /usr/bin/zip /usr/bin/unzip`
      - 解压：`#unzip x aaaa.zip`



### 2. VIM

- VIM的使用

  - Command mode:

    - `i`：enter insert mode
    - `x`：删除当前光标所在处的字符

  - Insert mode:

    - To get out of insert mode, hit the Escape key.

  - Last-line mode:

    - `q`：quit vim
    - `w`：save changes
    - `Esc`：quit last-line mode

  - ![figure_3_1](/Users/sissiliu/Desktop/figure_3_1.jpg)

  - 各种插入模式

    - `a`：在光标后插入
    - `o`：在当前行后插入一个新行
    - `O`：在当前行前插入一个新行

  - 简单的移动光标：

    - `0`：到行首
    - `^`：到本行第一个不是blank字符的位置
    - `$`：到本行行尾
    - `g_`：到本行最后一个不是blank字符的位置
    - `/pattern`：搜索pattern的字符串（如果搜索出多个匹配，可按n键到下一个）

  - Copy / paste:

    - `p`：粘贴（p/P都可以，p是表示在当前位置之后，P表示在当前位置之前）
    - `yy`：拷贝当前行于`ddP`???

  - Undo / redo:

    - `u`: undo
    - `<C-r>`: redo???

  - 打开/保存/退出/改变文件

    (Buffer)

    - `:e <path/to/file>` → 打开一个文件
    - `:w` → 存盘
    - `:saveas <path/to/file>` → 另存为 `<path/to/file>`
    - `:x`， `ZZ` 或 `:wq` → 保存并退出 (`:x` 表示仅在需要时保存，ZZ不需要输入冒号并回车)
    - `:q!` → 退出不保存 `:qa!` 强行退出所有的正在编辑的文件，就算别的文件有更改。
    - `:bn` 和 `:bp` → 你可以同时打开很多文件，使用这两个命令来切换下一个或上一个文件。（用`:n`到下一个文件）

  - 替换
    - 使用`:s` 命令来替换字符串
    - `:s/pid/ppid/` 替换当前行第一个 pid 为 ppid
    - `:s/pid/ppid/g` 替换当前行所有 pid 为 ppid
    - `:%s/pid/ppid/g`替换每一行中所有 pid 为 ppid

- Vim的配置

  - 基本配置：

    - `set nocompatible`：不与 Vi 兼容（采用 Vim 自己的操作命令）。
    - `syntax on`：打开语法高亮。自动识别代码，使用多种颜色显示。
    - `set showmode`：在底部显示，当前处于命令模式还是插入模式。
    - `set showcmd`：命令模式下，在底部显示，当前键入的指令。比如，键入的指令是`2y3d`，那么底部就会显示`2y3`，当键入`d`的时候，操作完成，显示消失。
    - `set mouse=a`：支持使用鼠标。
    - `set encoding=utf-8`：使用 utf-8 编码。
    - `set t_Co=256`：启用256色。
    - `filetype indent on`：开启文件类型检查，并且载入与该类型对应的缩进规则。比如，如果编辑的是`.py`文件，Vim 就是会找 Python 的缩进规则`~/.vim/indent/python.vim`。

  - 缩进：

    - `set autoindent`：按下回车键后，下一行的缩进会自动跟上一行的缩进保持一致。
    - `set tabstop=2`：按下 Tab 键时，Vim 显示的空格数。
    - `set shiftwidth=4`：在文本上按下`>>`（增加一级缩进）、`<<`（取消一级缩进）或者`==`（取消全部缩进）时，每一级的字符数。
    - `set expandtab`：由于 Tab 键在不同的编辑器缩进不一致，该设置自动将 Tab 转为空格。
    - `set softtabstop=2`：Tab 转为多少个空格。

  - 外观：

    - `set number`：显示行号

    - `set relativenumber`：显示光标所在的当前行的行号，其他行都为相对于该行的相对行号。

    - `set cursorline`：光标所在的当前行高亮。

    - `set textwidth=80`：设置行宽，即一行显示多少个字符。

    - `set wrap`：自动折行，即太长的行分成几行显示。

    - `set nowrap`：关闭自动折行

    - `set linebreak`：只有遇到指定的符号（比如空格、连词号和其他标点符号），才发生折行。也就是说，不会在单词内部折行。

    - `set wrapmargin=2`：指定折行处与编辑窗口的右边缘之间空出的字符数。

    - `set scrolloff=5`：垂直滚动时，光标距离顶部/底部的位置（单位：行）。

    - `set sidescrolloff=15`：水平滚动时，光标距离行首或行尾的位置（单位：字符）。该配置在不折行时比较有用。

    - `set laststatus=2`：是否显示状态栏。0 表示不显示，1 表示只在多窗口时显示，2 表示显示。

    - `set ruler`：在状态栏显示光标的当前位置（位于哪一行哪一列）



### 3. 程序调试Debugging

- GDB

  | 命令                                                     | 解释                                                         | 示例                                                         |
  | -------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
  | file                                                     | 加载被调试的可执行程序文件。因为一般都在被调试程序所在目录下执行GDB，因而文本名不需要带路径。 | (gdb) file gdb-sample                                        |
  | r                                                        | Run的简写，运行被调试的程序。如果此前没有下过断点，则执行完整个程序；如果有断点，则程序暂停在第一个可用断点处。 | (gdb) r                                                      |
  | c                                                        | Continue的简写，继续执行被调试程序，直至下一个断点或程序结束。 | (gdb) c                                                      |
  | b <行号> b <函数名称> b <函数名称> b <代码地址> d [编号] | b:Breakpoint的简写，设置断点。两可以使用“行号”“函数名称”“执行地址”等方式指定断点位置。其中在函数名称前面加“”，符号表示将断点设置在“由编译器生成的prolog代码处”。如果不了解汇编，可以不予理会此用法。 d: Delete breakpoint的简写，删除指定编号的某个断点，或删除所有断点。断点编号从1开始递增。 | (gdb) b 8 (gdb) b main (gdb) b *main (gdb) b *0x804835c (gdb) d |
  | s,n                                                      | s: 执行一行源程序代码，如果此行代码中有函数调用，则进入该函数； n:执行一行源程序代码，此行代码中的函数调用也一并执行。 s相当于其它调试器中的“Step Into (单步跟踪进入)”；n相当于其它调试器中的“StepOver (单步跟踪)”。这两个命令必须在有源代码调试信息的情况下才可以使用（GCC编译时使用“-g”参数）。 | (gdb) s (gdb) n                                              |
  | si,ni                                                    | si命令类似于s命令，ni命令类似于n命令。所不同的是，这两个命令（si/ni）所针对的是汇编指令，而s/n针对的是源代码。 | (gdb) si (gdb) ni                                            |
  | p <变量名称>                                             | Print的简写，显示指定变量（临时变量或全局变量）的值。        | (gdb) p i (gdb) p nGlobalVar                                 |
  | display undisplay <编号>                                 | display，设置程序中断后欲显示的数据及其格式。例如，如果希望每次程序中断后可以看到即将被执行的下一条汇编指令，可以使用命令“display /i pc其中pc其中pc 代表当前汇编指令， /i 表示以十六进行显示。当需要关心汇编代码时，此命令相当有用。 undispaly，取消先前的display设置，编号从1开始递增。 | (gdb) display /i $pc (gdb) undisplay 1                       |
  | i                                                        | Info的简写，用于显示各类信息，详情请查阅“help i”。           | (gdb) i r                                                    |
  | q                                                        | Quit的简写，退出 GDB调试环境。                               | (gdb) q                                                      |
  | help [命令名称]                                          | GDB帮助命令，提供对GDB名种命令的解释说明。如果指定了“命令名称”参数，则显示该命令的详细说明；如果没有指定参数，则分类显示所有GDB命令，供用户进一步浏览和查询。 | (gdb) help display                                           |

- Valgrind Memcheck
  - memcheck:这是valgrind应用最广泛的工具，一个重量级的内存检查器，能够发现开发中绝大多数内存错误问题，比如：使用未初始化的内存，使用已经释放了的内存，内存访问越界等。下面将重点介绍此功能。
  - callgrind: 主要用来检查程序中函数调用过程中出现的问题。
  - cachegrind: 主要用来检查程序中缓存使用出现的问题。
  - helgrind: 主要用来检查多线程程序中出现的竞争问题。
  - massif: 主要用来检查程序中堆栈使用中出现的问题。
  - extension: 可以利用core提供的功能，自己编写特定的内存调试工具。



### 4. Git

- Git常用命令

  | 命令                | 功能                               |
  | ------------------- | ---------------------------------- |
  | git init            | 初始化git管理的仓库                |
  | git add             | 添加到暂存区                       |
  | git commit          | 提交到仓库                         |
  | git status          | 查看当前仓库状态                   |
  | git diff            | 查看修改了的内容                   |
  | git log             | 查看仓库历史记录                   |
  | git relog           | 查看所有分支的所有操作记录         |
  | git checkout        | 切换分支                           |
  | git reset HEAD      | 回退到指定版本（HEAD表示最新版本） |
  | git reset HEAD file | 撤销暂存区的修改                   |
  | git rm              | 删除Git仓库文件                    |

- Example：

  - `mkdir learngit`：创建一个 learngit 目录作为工作区
  - `git init`：初始化 git 管理的仓库，显示初始化成功learngit目录下有`.git`隐藏文件夹，为本地版本库。
  - `echo "This is first write" > readme.txt`：新建一个 readme.txt 文件，写入了"This is first write"。
  - `git add readme.txt`：添加readme.txt文件到暂存区。
  - `git status`：查看当前仓库的状态，显示readme.txt 的改变等待提交。
  - `git commit –m "wrote a readme.txt file"`：提交文件到版本库并且附带注释 "wrote a readme.txt file"
  - `git diff`：显示修改了的文件的信息。
  - `git status`：显示当前没有需要提交的。
  - `git log`：查看仓库历史记录,显示提交的 id 号，作者和日期。
  - `git checkout -- readme.txt`：撤销`readme.txt`文件的修改。
  - `git status`：查看当前仓库的状态。
  - `git rm readme.txt`：删除`readme.txt`文件

- Gitlab服务器

  - 用命令`git clone`克隆一个本地库:

    ```shell
    git clone git@github.com.cn:michaelliao/gitskills.git
    ```

  - 分支管理

    | 命令                   | 描述                     |
    | ---------------------- | ------------------------ |
    | git checkout -b 分支名 | 创建新分支并切换到该分支 |
    | git branch             | 查看当前的分支           |
    | git add 文件名         | 添加文件                 |
    | git commit 文件名      | 提交文件                 |
    | git merge 分支名       | 合并分支                 |
    | git branch -d 分支名   | 删除分支                 |
    | git stash              | 把当前工作现场“储藏”起来 |
    | git remote             | 查看远程库的信息         |
    | git push origin 分支名 | 推送修改到分支上         |
    | git pull               | 把最新提交的内容抓取下来 |



### 5. Makefile

- Makefile文件：

  - 自动化编译器配置文件可以起以下名字：Makefile或makefile

    **注意：不能写成MakeFile**（在Linux命令行提示符输入make，它会在当前目录下按顺序寻找GNUmakefile、Makefile和makefile，如未找到则报错，找到则把Makefile文件中的第一个目标作为最终目标）

- 清空目标文件的规则

  每个Makefile中都应该写一个清空目标文件（.o和执行文件）的规则，这不仅便于重编译，也很利于保持文件的清洁。一般的风格都是

  ```makefile
  clean:
  rm myapp $(objects)
  ```

  使用命令`make clean`就会删除`clean`规定的内容

- CMake：Cmake是一个编译、构建工具。使用CMakeLists.txt来描述构建过程，可以生成标准的构建文件，如Makefile。一般先编写CMakeLists.txt，然后通过cmake来生成Makefile，最后执行make进行编译。

- CMake的使用流程：

  - 编写`CMakeLists.txt`
  - 执行`cmake`，一般在工程根目录下`mkdir build;cd build;`然后执行`cmake .`这样的目的是为了保证源码不会污染
  - 执行`make`进行编译

- catkin_make VS. cmake

  - CMake标准流程：

    ```shell
    $ mkdir build  
    $ cd build  
    $ cmake ..  
    $ make  
    $ make install  # (可选)  
    ```

  - catkin_make的流程

    ```shell
    # In a catkin workspace  $ catkin_make  
    $ catkin_make install  # (可选)  
    如果源码不在默认工作空间,需要指定编译路径:  
      
    # In a catkin workspace  
    $ catkin_make --source my_src  
    $ catkin_make install --source my_src  # (optionally) 
    ```

