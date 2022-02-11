---
title: 📒【Markdown】MarkDown简明语法
date: 2020-02-10
categories: 计算机基础
comments: true
tag: Markdown
toc: true
---

**对于新手使用markdown语法时，要注意在每一个标记的后面加上一个空格**
# 1.分级标题

使用===表示一级标题，使用---表示二级标题
或者使用在行首加#号表示不同级别的标题
    
    # 这是一个一级标题
    ## 这是一个二级标题
    ### 这是一个三级标题
    #### 这是一个四级标题
    共有六个不同分级的标题


# 2.粗体和斜体

使用 * 表示斜体，使用 ** 表示粗体,使用 *** 表示加粗斜体，使用 ~~ 表示删除线
*这是斜体文字*
**这是粗体文字**
***这是加粗斜体***
~~这是删除线~~

# 3.列表
### 3.1有序列表
使用数字和小数点表示
1. 第一点
2. 第二点
3. 第三点

### 3.2无序列表
使用*、+、-表示无序列表
示例：
* 无序列表 一（用*表示
+ 无序列表 二（用+表示
- 无序列表 三（用-表示

# 4.文字引用
使用>表示文字引用
> 这是一段引用文字

区块引用可以嵌套
> 引用
>> 再引用

# 5.代码块
### 5.1行内代码块
使用 \`代码`（英文输入，数字1的左侧）表示行内代码块

`printf("Hello world.")`
### 5.2整体代码块
    四个缩进空格表示整体代码块
    或者使用```多行内容```

# 6.链接地址
使用 \[写入需要链接的文字](需要链接的地址) 作为行内链接方式
[Hanwin的个人博客](hanwinbi.github.io)

使用 \[这里添入链接文字]\[链接标记1]、\[这里添入链接文字]\[链接标记2]作为参考式使用
链接地址放在最后一行
\[链接标记1]: hanwinbi.github.io
\[链接标记1]: hanwinbi.github.io

使用 <> 自动链接简单地址 
<https:hanwinbi.github.io>

# 7.锚点
当前Markdown引擎不可用（直接使用的主题提供的toc

# 8.插入图片
使用\!\[加载不出时显示的文字](图片地址 "悬浮时显示的文字")作为行内插图

![Loading](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/2019-12-27.jpg "Home") 

类似链接地址\![图片][链接标记3]
\[链接标记3]: https:图片地址 "title"

# 9.注脚
在需要添加注脚的文字后[^注脚标记],注脚自动添加到段落最后[^2]

\[^注脚标记]: hello
\[^2]: 不同标记形式的注脚
[^注脚标记]: 注脚标记内容 
[^2]: 不同标记形式的注脚

# 10.表格
1. 不管是哪种方式，第一行为表头，第二行分隔表头和主体部分，第三行开始每一行为一个表格行。  
2. 列于列之间用管道符|隔开。原生方式的表格每一行的两边也要有管道符。    
3. 第二行还可以为不同的列指定对齐方向。默认为左对齐，在-右边加上:就右对齐。

```
表头1|表头2|表头3|表头4|表头5
-|-:|-|-|-|
左对齐文字|右对齐文字
```

表头1|表头2|表头3|表头4|表头5
-|-:|-|-|-|
左对齐文字|右对齐文字

# 11.LaTeX公式
使用 `\\(` `\\)` 表示行内公式，例如质能方程：\\(E=mc^2\\)
使用 `$$begin{split}` `\end{split}$$`在使用MathJax的时候才能正常渲染

示例：
```
行内显示
\\(q \in R \\)
\\(\mathcal{F}(x)=\mathcal{H}(x)-x\\)
\\(\mathcal{C}\phi \delta e \mathfrak{M}\alpha th \mathit{I}n \mathcal{H}ex\sigma \mathbb{N}o\omega!\\)
```

\\(q \in R \\)
\\(\mathcal{F}(x)=\mathcal{H}(x)-x\\)
\\(\mathcal{C}\phi \delta e \mathfrak{M}\alpha th \mathit{I}n \mathcal{H}ex\sigma \mathbb{N}o\omega!\\)

```
$$\begin{split}
\lim_{n\rightarrow \infty}(1+2^n+3^n)^\frac{1}{x+\sin n}
\end{split}$$

$$\begin{split}
\frac{\partial{\mathcal{E}}}{\partial{x_l}} & = \frac{\partial{\mathcal{E}}}{\partial{x_L}}\frac{\partial{x_L}}{\partial{x_l}}\\\\
& = \frac{\partial{\mathcal{E}}}{\partial{x_L}}\Big(1+\frac{\partial{}}{\partial{x_l}}\sum_{i=l}^{L-1}\mathcal{F}(x_i,\mathcal{W}_i)\Big)
\end{split}$$
```

$$\begin{split}
\lim_{n\rightarrow \infty}(1+2^n+3^n)^\frac{1}{x+\sin n}
\end{split}$$

$$\begin{split}
\frac{\partial{\mathcal{E}}}{\partial{x_l}} & = \frac{\partial{\mathcal{E}}}{\partial{x_L}}\frac{\partial{x_L}}{\partial{x_l}}\\\\
& = \frac{\partial{\mathcal{E}}}{\partial{x_L}}\Big(1+\frac{\partial{}}{\partial{x_l}}\sum_{i=l}^{L-1}\mathcal{F}(x_i,\mathcal{W}_i)\Big)
\end{split}$$

[查看更多LateX语法](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)

[解决博客LateX渲染问题](https://linkinpark213.com/2018/04/24/mathjax/)

# 12.流程图

# Tips
在段落前使用[TOC]显示全文目录结构