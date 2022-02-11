---
title: 📒【汇编原理】汇编原理第8-9章笔记
date: 2021-06-10
categories: 计算机基础
comments: true
tag: Assembly
toc: true
---

## 第八章 数据处理的两个问题
1. 处理的数据放在什么地方？
2. 要处理的数据有多长？

在`[...]`中，`bx si di bp`可以单独出现，`bx bp`只能和`si di`组合出现。如果`[...]`中使用的是 `bp` 寄存器，如果没有指明段地址，段地址会默认在 `ss` 中

汇编语言中数据位置的表达：
- 立即数
	- `mov ax,1`
	- `add ax,2000h`
- 寄存器
- 段地址（SA）和偏移地址（EA）

寻址方式
![寻址方式小结](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/assembly/register_6.png)

指令要处理的数据有多长
- 通过寄存器名指明要处理的数据的尺寸
- 在没有寄存器名存在的其况下，用操作服 `X ptr`指明内存单元的长度，X在汇编指令中可以为 word 或 byte
	- `mov word ptr ds:[0],1`
	- `mov byte ptr ds:[0],1`
push 指令只进行字操作，不能指定是字单元还是字节单元。

### div指令
1. 除数：8位和16位，在寄存器或内存单元中
2. 被除数：默认存放在 AX 或 DX 和 AX 中，除数为8位，被除数为 16 位；除数为 16 位，被除数则为 32 位，在 DX 和 AX 中存放，DX 放高 16 位，AX 放低 16 位
3. 结果：除数为 8 位， AL 存储商，AH 存储余数；除数是 16 位， AX 存储商，DX 存储余数
原始数据放在AX或 DX 和 AX 中，除以一个寄存器或内存单元的数据之后会将结果放在AX 或 DX 和 AX 中。

### 伪指令 dd
db 字节型数据
dw 字型数据
dd 是用来定义 dword（double word，双字）型数据的。32位

### dup
dup 是一个操作符，在汇编语言中同 `db dw dd` 一样，由编译器识别处理的符号，和他们配合使用，进行数据重复。
`db repeat_times dup (data)、dw repeat_times dup (data)、dd repeat_times dup (data)`

### 实验七
这个实验我在网络上找了一些相关的答案，但是和我的答案有些出入，目前自认为是正确的，如果有错误，希望大佬指出。
注：在使用`d 0778:0`展示运行后的数据，其中只有年份能够正常的显示，这应该是对的，它并不能展示双字型，观察字节数据即可
```
assume cs:codesg,ds:datasg,ss:stacksg,es:table

datasg segment

    db '1975','1976','1977','1978','1979','1980','1981','1982','1983'
    db '1984','1985','1986','1987','1988','1989','1990','1991','1992'
    db '1993','1994','1995' ;84 byte 21*4
    
    dd 16,22,382,1356,2390,8000,16000,24486,50065,97479,140417,197514
    dd 345980,590827,803530,1183000,1843000,2759000,3753000,4649000,5937000
    ;21年公司总收入的21个dword型数据 21 * 4 = 84 byte

    dw 3,7,9,13,28,38,130,220,476,778,1001,1442,2258,2793,4037,5635,8226
    dw 11542,14430,15257,17800  
    ;21年公司雇员人数 21 * 2 = 42 byte

datasg ends

table segment
    db 21 dup ('year summ ne ?? ')
table ends

stacksg segment
    dw 10 dup(0)
stacksg ends

codesg segment
    start:  mov ax, datasg
            mov ds, ax
            mov ax, table
            mov es, ax

            mov cx, 11
            mov bx, 0       
            mov si, 84      ;收入数据开始位置
            mov di, 168     ;雇员数数据开始位置
            mov bp, 0       ;年份数据开始位置
        s:  ;先复制年份
            mov ax, ds:[bp]
            mov es:[bx+0], ax
            mov ax, ds:[bp+2]
            mov es:[bx+2], ax
            add bp, 4
            ;处理收入
            mov ax, ds:[si]
            mov es:[bx+5], ax
            mov ax, ds:[si+2]
            mov es:[bx+7], ax
            add si, 4
            ;处理雇员数
            mov ax, ds:[di]
            mov es:[bx+10], ax
            add di, 2
            ;处理人均收入
            mov ax, es:[bx+5]
            mov dx, es:[bx+7]
            div word ptr es:[bx+10]
            mov es:[bx+13], ax
            add bx, 16
            loop s

    mov ax,4c00h
    int 21h

codesg ends

end start
```

---
## 第九章 转移指令的原理
可以修改`IP`，或同时修改`CS`和`IP`的指令统称为转移指令
转移行为分类：
- 只修改`IP`的段内转移
	- 段转移 IP的修改范围-128～127
	- 近转移 -32768～32767
- 同时修改`CS`和`IP`的段间转移

转移指令分类
- 无条件转移指令 `jmp`
- 条件转移指令
- 循环指令 `loop`
- 过程
- 中断

`offset`：由编译器处理的符号，取得标号的偏移地址

jmp是如何实现转移的?
`jmp short s`对应的机器码是`EB03`，在机器码中没有存储要跳转的目的地址，而是通过偏移地址的方式，03就是偏移的距离，用当前地址减去偏移的距离就是跳转的位置，`jmp short/near/far s`的不同之处就是可以跳转的距离不同（short、near不更改段地址通过偏移距离转移，far需要段地址）。位移由编译程序在编译时算出

`jmp short 标号`：转到标号处执行指令，实现的是段内短转移
`jmp  near ptr 标号`：段内近转移
`jmp far ptr 标号`：指明用标号的段地址和偏移地址修改`CS`和`IP`

转移地址在寄存器中的时候使用 `jmp 16位reg`

转移地址在内存中的时候
1. `jmp word ptr 内存单元地址`段内转移 
2. `jmp dword ptr 内存单元地址`段间转移，高位是段地址，低位是偏移地址
`jcxz 标号`（jump cx == zero）：有条件转移指令，短转移。所有的有条件转移指令都是段转移。if(cx == 0)转移到标号处进行
`loop`：短转移，所有循环指令都是短转移

编译器对转移位越界时，编译器会报错。
#### 检测点9.1
1) 第一个字节为任意数据，第二个字节是offset start，所以答案可以是`db 3 dup(0)`
2) `bx cs`
3) CS = 0006h IP = 00BEh

#### 检测点9.2
```
mov cl, ds:[bx]
mov ch, 0
jcxz ok
inc bx
```

#### 检测点9.3
`add cx, 1` 或`inc cx`

### 实验8
```
assume cs:codesg

codesg segment
    mov ax, 4c00h
    int 21h

    start: mov ax, 0
    s:  nop	;占一个字节	;jmp short s1(EBF6) 偏移10个字节，到代码段开始的地方
        nop			

        mov di, offset s
        mov si, offset s2
        mov ax, cs:[si]
        mov cs:[di], ax	;将S2的第一条语句复制到S的两个nop处，下一句执行s0
    
    s0: jmp short start	;跳转到标号start
    
    s1: mov ax, 0	
        int 21h
        mov ax, 0

    s2: jmp short s1
        nop
        
codesg ends
end start
```
可以正常返回

### 实验9
```
assume cs:codesg

datasg segment
    db 'welcome to masm!'
    db 00000010b,00100100b,01110001b
datasg ends

stacksg segment
    db 10 dup(0)
stacksg ends

codesg segment

    start:  mov ax, 0b800h
            mov es, ax ;显示区域
            mov si, 524h    ;第八行
            mov bx, 0   ;数据位置
            mov cx, 3   ;循环次数
            mov di, 16  ;属性位置

            mov ax, datasg
            mov ds, ax

            mov ax, stacksg
            mov ss, ax

    s:      push cx
            mov cx, 16

    disp:   mov al, ds:[bx] ;字符
            mov ah, ds:[di] ;属性
            mov es:[si], ax 
            inc bx 
            add si,2
            loop disp

            mov bx, 0
            inc di
            add si, 128
            pop cx
            loop s        
            mov ax, 4c00h
            int 21h

codesg ends
end start
```