---
title: C++程序内存泄漏及性能检测
date: 2024-10-01T22:08:28+08:00
categories: [C++]
tags: [C++,Utils]
---

## 一、安装
``` bash 安装valgrind及massif-visualizer可视化分析工具
#安装 valgrind
sudo apt-get install valgrind  # Debian/Ubuntu
sudo yum install valgrind      # CentOS/Fedora
#安装 massif-visualizer
sudo apt install massif-visualizer
```
## 二、检测内存泄漏
### 1.基本介绍
>- （1）Memcheck。这是valgrind应用最广泛的工具，一个重量级的内存检查器，能够发现开发中绝大多数内存错误使用情况，比如：使用未初始化的内存，使用已经释放了的内存，内存访问越界等。这也是本文将重点介绍的部分。
>- （2）Callgrind。它主要用来检查程序中函数调用过程中出现的问题。
>- （3）Cachegrind。它主要用来检查程序中缓存使用出现的问题。
>- （4）Helgrind。它主要用来检查多线程程序中出现的竞争问题。
>- （5）Massif。它主要用来检查程序中堆栈使用中出现的问题。
>- （6）Extension。可以利用core提供的功能，自己编写特定的内存调试工具
>- （7）其他 **valgrind插件用法: https://blog.csdn.net/weixin_45518728/article/details/119865117** 


### 2.使用valgrind
``` bash 
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes xxx args ...
```
进程退出后，会生成日志信息，可查看内存泄漏情况及泄漏位置
### 3.使用valgrind+massif
```bash
# 生成massif.out.xxx文件
valgrind --tool=massif xxx args ...
#使用massif-visualizer查看进程内存增长
massif-visualizer massif.out.12345
#或 
ms_print massif.out.12345 
```
![这是图片](/images/massif.png "内存泄漏检测")
### <mark> 4.为何需要两种方式检测？<mark>
>
>- 第一种方式，进程调用某库函数分配内存不释放， 但是在进程退出时，统一释放内存，valgrind 检查不出来
>- 第二种方式，使用massif工具进行全过程检测，可有效跟踪服务内存不断增长的情况