---
layout:     post
title:      "内存池"
subtitle:   " \"手撕内存池\""
date:       2024-05-01 10:00:00
author:     "Chenyongquan2"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - C++
    - 内存池
    - Memorypool
---


# 内存池实现
文档说明
[記憶池 (jonny.vip)](https://jonny.vip/2019/07/16/%E8%A8%98%E6%86%B6%E6%B1%A0/)

代码实现
[chenyongquan2/memory_pool: the implement of memory pool with c++ (github.com)](https://github.com/chenyongquan2/memory_pool)


## 重要实现细节
### 模板使用
#### 模板参数
本身不是一个类型，而是一个变量参数，可以用来修饰类，也可修饰函数，见下文：
![[Pasted image 20240502171512.png]]


####  初始化模板类的静态成员
![[Pasted image 20240502174216.png]]



#### 实例化类的模板函数

![[Pasted image 20240502174401.png]]

### union的使用
![[Pasted image 20240502171659.png]]

### sizeof
![[Pasted image 20240502171830.png]]

### constexpr
#### constexpr static auto 修饰变量
![[Pasted image 20240502171939.png]]


#### constexpr修饰函数
![[Pasted image 20240502172933.png]]


#### if constexpr 实现代码的条件编译
![[Pasted image 20240502174431.png]]



### noexpect 修饰函数
表示该函数不会抛出异常，编译器你就不用给我去生成一些对异常去处理捕获的代码了，
这样我运行起来就不用执行这些代码，运行速度就不会得到降低

![[Pasted image 20240502173712.png]]


### volatile
防止多线程场景下由于对变量的读取去做缓存优化，直接去寄存器读，而没有用内存去读，而内存的值是被别的线程去修改后的最新值，而寄存器的可能是老值。用此关键词，每次去访问都会从内存里读，而不是寄存器(结合汇编的角度去理解)
![[Pasted image 20240502172242.png]]


### 指针相减的类型ptrdifff_t类型与size_t
![[Pasted image 20240502172825.png]]



### Todo: new和delete的重载函数 如何new失败不跑出异常+定位new版本/ placement new

![[Pasted image 20240502173817.png]]
小技巧，去调用的时候，把operator new当做一个函数名的整体看待即可
eg:

![[Pasted image 20240502174452.png]]




### {} 初始化变量
![[Pasted image 20240502174826.png]]


### auto& 去修改数组本身
![[Pasted image 20240502175045.png]]


### reinterpret_cast来完成没有直接转换关系的指针类型转换
例如我们这里要把char* 给转成 T *
本来想用static_cast,但是sattic_cast会在编译期去做类型安全性检查滴，因此这里直接报错了。

![[Pasted image 20240502175324.png]]
![[Pasted image 20240502175409.png]]


### 位运算

![[Pasted image 20240502175652.png]]

![[Pasted image 20240502175706.png]]


### 一个c++程序调用的detele把内存给释放掉之后，会立马被操作系统给回收吗？我再通过new去向操作系统申请，是否可能返回一样的内存地址呢
![[Pasted image 20240502184522.png]]



## clangFormat的引入
在项目的根目录下加入一个名字叫.clang.format的文件
内容如下:
```
BasedOnStyle: webkit

ColumnLimit:     120

SortIncludes: false

AlwaysBreakTemplateDeclarations: Yes
```

![[Pasted image 20240502172300.png]]





## 单元测试
相关的判断方法
![[Pasted image 20240502184635.png]]




## CI/CD
[基于 Github Action 的 CI/CD 流程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/250534172)

相当于github给你提供了一台虚拟机器，你可以在上面去下载配置环境依赖，生成各个平台的程序&打包程序，单元测试，也可以用来去执行一些自动化任务

### GitHub Actions

Market ref
[GitHub Marketplace · Actions to improve your workflow](https://github.com/marketplace?type=actions)


![[Pasted image 20240502185341.png]]


![[Pasted image 20240502185405.png]]

#### 相关语法


![[Pasted image 20240502185515.png]]
![[Pasted image 20240502185559.png]]![[Pasted image 20240502185625.png]]![[Pasted image 20240502185707.png]]

ref:
[sdras/awesome-actions: A curated list of awesome actions to use on GitHub](https://github.com/sdras/awesome-actions)


[GitHub Marketplace · Actions to improve your workflow](https://github.com/marketplace?type=actions)



### git tag的知识

