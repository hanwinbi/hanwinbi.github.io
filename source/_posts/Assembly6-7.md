---
title: 📒【汇编原理】汇编原理第6-7章笔记
date: 2021-06-07
categories: 计算机基础
comments: true
tag: Assembly
toc: true
---

## 第六章
`dw` 的含义是定义字符型数据，即`define word`

数据和代码段放在一块之后会导致代码执行并不是从指令开始，而是从数据开始，需要手动设置指向第一条指令。另一种操作方式是在程序的第一条指令前面加上一个标号`start`，这个标号还需要在标号end之后出现
```
assume cs:code
code segment

	dw data //数据
	start: .... //程序入口
	
code ends
end start //程序结束
```
根据什么设置CPU的`CS:IP`指向程序的第一条指令？用伪指令 end 描述程序的结束和程序的入口。在编译连接之后，由 `end start` 指明程序的入口，被转换成一个入口地址，存储在可执行文件的描述信息中。

#### 检测点 6.1
1) `mov cs:[bx],ax`
2) cs 24h `pop cs:[bx]`
 
将代码、数据、栈放在一个段中，在体量比较大的程序中会比较混乱，可以使用不同的段来存放这些数据，每个段的段名不一样。

assume的作用：将定义的具有一定用途的段和相关的寄存器联系起来

```
assume cs:code,ds:data,ss:stack

data segment	;数据段
...
data ends

stack segment	;栈段	
...
stack ends

code segment	;代码段
	start:	...
code ends

end start
```


### 实验五
数据段和栈段在程序加载后实际占据的空间都是以16个字节为单位的。如果不足，以0补全填充。
程序最开始时ds：00~ds：100H是留给程序与操作系统通讯使用的psp内存段，data
数据段从ds:100H处开始
1) a. 数据段中的数据保持不变
b. 076C 076B 076A
c. 和答案不一样(X-2 X-1)
2) a. 数据段中的数据保持不变
b. 076C 076B 076A
c. 和答案不一样(X-2 X-1)
3) a. 数据段中的数据保持不变
b. cs=076A ss=0769 ds=075A
c. 和答案不太一样

---
## 第七章 更灵活的定位内存地址的方法
and 和 or 指令，逻辑与(1 && 1 = 1)和逻辑或(1 || 0 =1)

使用更方便的方式指明内存单元`[bx+idata] 或 [idata+bx]、idata[bx]、[bx].idata`
##### Tips:你意想不到的事情
指明内存单元的方式可以用`[bx+idata] 或 [idata+bx]、idata[bx]、[bx].idata`，C语言中的数组是由数组名和下标，形如`a[0]`构成，`a`其实是数组指针，指向数组的第一个元素，`[idata]`中的就是偏移地址，偏移的大小是`sizeof()*idata`，既然汇编语言中可以表示为`idata[bx]`的形式，那么我们可不可以把数组获取数组中数据也表示成类似的形式`idata[数组名]`。
我给出了下面的例子
```c++
#include <iostream>
#include <stdlib.h>
using namespace std;

int main(){
	int a[5] = {0, 1, 2, 3, 4};
	for(auto &i:a){
		cout << i[a] << endl;
	}
}
```
运行之后你就会发现这竟然是可以正常运行的，而且结果是正确的！！C语言在表示上和汇编语言有着相似性。

`SI`和`DI`类似于 bx 的功能，不能分成两个 8 位寄存器来使用

`[bx+si] [bx+di]`
`[bx+si+idata] [bx+di+idata] [bx][si].idata idata[bx][si]`

> 一般来说，在需要暂存数据的时候，我们应该使用栈。

### 实验六
```
assume cs:codesg,ds:datasg,ss:stacksg

stacksg segment
    dw 0,0,0,0,0,0,0,0
stacksg ends

datasg segment
    db '1. file         '
    db '2. edit         '
    db '3. search       '
    db '4. view         '
    db '5. option       '
    db '6. help         '
datasg ends

codesg segment
    start:  mov ax,stacksg
            mov ss,ax
            mov sp,16
            mov ax,datasg
            mov ds,ax
            mov bx,0

            mov cx,6
        s0: push cx
            mov cx, 4
            mov si, 3

        s:  mov al,[bx+si]
            and al,11011111b
            mov [bx+si],al
            inc si
            loop s
            
            add bx, 16
            pop cx
            loop s0

        mov ax, 4c00h
        int 21h
codesg ends
end start
```