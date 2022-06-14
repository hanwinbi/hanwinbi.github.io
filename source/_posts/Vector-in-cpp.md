---
title: 📒【STL】Vector向量
date: 2020-02-15
catagories: 计算机基础
tag: C++
comments: true
---

参考书籍：《C++标准程序库》

### vector定义:

>C++中的一种数据结构,确切的说是一个类.它相当于一个动态的数组,当程序员无法知道自己需要的数组的规模多大时,用其来解决问题可以达到最大节约空间的目的。

### 用法:

1. 头文件#include <vector>以包含所需要的类文件vector
2. vector是定义于namespace std内的template，一定要加上using namespace std;

# Vector的特性及基本操作函数

- Vector支持随机存取
- 在末端添加或删除元素，性能好；在前端或中部插入和删除，性能不太好

### 操作函数

**大小和容量**
size()
empty()
max_size()
capacity()：返回vector世纪能够容纳的元素数量，超过这个数量，vector就会重新配置内部存储器

**避免重新配置内存**

reserve():保留适当容量，避免一再重新配置内存，和vectr相关的reference、pointer、iterators就不会失效

初始化期间就向构造函数传递附加参数：vector<T> v(5);如果类型复杂，那么初始化操作会很费时

如果调用的reserve()所给的参数比当前vector的容量还小，不会引发任何反应

但是可以间接缩小vector的大小。交换两个vector，内容和容量都会交换：

```c++
void shrinkCapacity(std::vector<T> v){
	std::vector<T> tmp(v);
	v.swap(tmp);
}//所有的reference、pointer、iterators依然会指向原来的对象，所以会失效

或者直接使用std::vector<T>(v).swap(v);
```

Vectors的构造函数和析构函数

![Vectors的构造函数和析构函数](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vectors%E7%9A%84%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%E5%92%8C%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0.png)

Vectors的非变动性操作

![Vectors的非变动性操作](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vectors%E7%9A%84%E9%9D%9E%E5%8F%98%E5%8A%A8%E6%80%A7%E6%93%8D%E4%BD%9C.png)

Vectors的赋值操作

![Vectors的赋值操作](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vectors%E7%9A%84%E8%B5%8B%E5%80%BC%E6%93%8D%E4%BD%9C.png)

**元素存取**

Vector允许直接存取元素

![Vector允许直接存取元素的各项操作](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vectors%E7%9A%84%E8%B5%8B%E5%80%BC%E6%93%8D%E4%BD%9C.png)

**迭代器相关函数**

Vectors的迭代器相关函数

![Vectors的迭代器相关函数](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vectors%E7%9A%84%E8%BF%AD%E4%BB%A3%E5%99%A8%E7%9B%B8%E5%85%B3%E5%87%BD%E6%95%B0.png)

**insert和remove元素**

1. 迭代器必须指向一个合法位置
2. 区间的起始位置不能在结束为止之后
3. 不能从空容器中移除元素

Vector的安插、移除相关操作

![Vector的安插、移除相关操作](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/Vector%E7%9A%84%E5%AE%89%E6%8F%92%E7%A7%BB%E9%99%A4%E7%9B%B8%E5%85%B3%E6%93%8D%E4%BD%9C.png)

Vector为提供任何函数可以直接移除“与某值相等”的所有元素，可以如下操作：

```c++
std::vector<Elem> coll;
…
coll.erase(remove(coll.begin(),coll,end(),val),coll.erd());
```

如只是移除“与某值相等”的第一个元素，可以这么做：

```c++
std::vector<Elem> coll;
…
Std::vector<Elem>::iterator pos;
Pos = find(coll.begin(),coll,end(),
		val);
if(pos != coll.end()){
	coll.erase(pos);
}
```

将Vector当作一般的Arrays使用

对于任意的vector vVS任意一个合法索引i，以下的表达式肯定为true：

`&v[i] = &v[0] + i;`

所以在任何地点只要你需要一个动态数组，你就可以使用vector。



例：利用vector来存放常规的c字符串

```c++
std::vector<char> v;
v.resize(41);
strcpy(&v[0],”hello world”);
printf(“%s\n”,&v[0]);
```

Notes:以上必须确保vector的大小足以容纳所有元素。之歌例子说明，不管出于什么原因，只要你需要一个元素类型为T的数组，就可以利用vector<T>,然后传递第一个元素的地址给它。

千万不要吧迭代器当作第一个元素来传递。vector迭代器也许不是一个一般指针

```c++
printf(“%s\n”,v.begin());//ERROR
printf(“%s\n”,&v[0]);//OK
```



## 异常处理

*见文档 p.156

## Class vector\<bool>

STL为元素类型为bool的vector设计了一个特殊版本，目的是获得一个优化的vector。

一般的vector会为每一个元素至少分配一个byte空间，而vector\<bool>特殊版本我不只会用一个bit来存储一个元素（用0、1表示真假，fantastic！），只占原来的1/8，但是C++的最小寻址空间通常以byte为单位，所以要对上述的vector特殊版本真的不reference和iterator做特殊考虑

vector\<bool>提供某些特殊的bit操作（Amazing！），可以利用他们更方便的操作bit或标志（flags）

![Vector<bool>的特殊操作](https://github-ihs-1257225145.cos.ap-shanghai.myqcloud.com/C%2B%2B/vectorbool%E7%9A%84%E7%89%B9%E6%AE%8A%E6%93%8D%E4%BD%9C.png)

你可以对单一bool元素调用flip()。也许你觉得让下标操作符返回bool，再对基本类型调用flip()是不可能的。然而这里vector\<bool>用了一个常用技巧，称作proxy。（最后一点点不太懂了，就没看了