---
title: 📒【Makefile】初试 Makefile
date: 2022-04-20
categories: 计算机基础
comments: true
tag: makefile
toc: true
---

## make 工具
编译项目的时候每次都要手打编译命令非常的繁琐，这些重复性的工作可以使用 `make` 工具来完成。程序员写好每一个模块需要创建的内容和依赖，这些模块间被组合起来用来创建程序。`make`可以用来管理创建程序必要的执行命令（编译、链接、载入等）。`make` 和相应的 `makefile` 实际上非常易学。

## 编写 makefile
### 变量
一个 makefile 文件包含许多「变量」和「依赖」。一个变量定义代表文本字符串，类似于 C章编译器中预处理器的宏替换功能。变量通常用于代表一组需要搜索的目录、编译器可选参数、需要运行的程序的名称等。例如:
```
CC = g++
```
会创建一个叫做 `CC` 的变量，被赋值为 `g++`，通常变量名都设置为大写字母，变量名大小写敏感。你可以定义一些自己的变量名，有一些约定俗称的变量名如，`CC, CFLAGS, LDFLAGS`
- `CC` : C 编译器的名称，大多数版本的 make 默认设置为 cc，确保自己设置为 `gcc` 或者 `g++`
- `CFLAGS` : 用于编译源文件的可选参数。通常用于设置 include 的路径和非标准目录或者构建 debug 版本，`-I` 和 `-g` 编译标志。也可以写为 `CPPFLAGS` 用于 `C++` 程序。
- `LDFLAGS` : 传递链接器的可选参数。通常用于设置非标准路径下的库搜索和包含应用特定的苦文件， `-L` 和 `-l` 编译标志。
变量的使用方法参考，
```
CFLAGS = -g -I/usr/class/cs193d/include
$(CC) $(CFLAGS) -c binky.c
```
第一行变量 CFLAGS 打开 debug 信息和添加目录到 include 文件的搜索路径。如果使用了一个没有在 makefile 中定义的变量，make 会设置为一个空串。

### 依赖/编译规则
给出一组文件，当其中文件内容发生改变时，按照制定的规则告诉编译器重新生成目标文件。除了第一个规则是默认规则外，makefile 中规则的顺序没有区别。一个规则通常包含两行，一行依赖，一行命令，例如：
```
binky.o: binky.c binky.h akbar.h
	$(CC) $(CFLAGS) -c binky.c
```
第一行 `:` 后任一文件`binky.c, binky.h, akbar.h` 发生改变时， `binky.o` 都需要发生重新编译，也就是说目标文件 `binky.o` 依赖于这三个文件。
第二行列出了重建 `binky.o` 文件必须执行的命令。这一行必须要一个 `<tab>` 缩进字符。

下面是我在编写 `echoserver` 时学习了 `makefile` 后编写的内容，
```
CC = g++

SRC_SERVER = echoserver.cpp
SRC_CLIENT = echoclient.cpp
CPPFLAGS = -g -O0 -Wall
OBJS = server.o client.o

SERVER = server
CLIENT = client

$(SERVER): $(OBJS)
	$(CC) -o $(SERVER) $(CPPFLAGS) $(SRC_SERVER)
	$(CC) -o $(CLIENT) $(CPPFLAGS) $(SRC_CLIENT)

clean:
	rm *.o $(SERVER) $(CLIENT)

server.o : echoserver.cpp
client.o : echoclient.cpp
```
