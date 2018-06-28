---
layout: post
title: Linux头文件版本号
date: 2018-06-28
categories: Linux
tags: Linux
---

**重点：version.h中的版本号以十进制显示，需要转换为二进制查看**

Linux的内核头文件一般存储在：
<pre><code>/usr/include/linux/version.h
</code></pre>

其内容一般为：
<pre><code>#define LINUX_VERSION_CODE 132640
#define KERNEL_VERSION(a,b,c) (((a) << 16) + ((b) << 8) + (c))
#define RHEL_MAJOR 6
#define RHEL_MINOR 9
#define RHEL_RELEASE_VERSION(a,b) (((a) << 8) + (b))
#define RHEL_RELEASE_CODE 1545
#define RHEL_RELEASE 695
#define RHEL_16KSTACK_BUILD 520
</code></pre>

其中132640的二进制数为100000011000100000
根据

<pre><code>(a,b,c) (((a) << 16) + ((b) << 8) + (c))</pre></code>

取走16位0000011000100000得10，十进制为2；

取走8位00100000得00000110，十进制为6；

剩下的00100000，十进制为32，可以判断其为Lilnux 2.6.32。

这个问题是在安装最新的Gblic (glibc-2.27)时发现的，其需要至少Linux 3.2.0的版本。

通过查找Gblic与Linux 内核的关系，找到glibc-2.20安装。查找对应关系的网址如下：
https://sourceware.org/glibc/wiki/Glibc%20Timeline

后记：一开始我误以为是16进制的计算，算错了版本，发现和/proc/version的不符。经过朋友的指点，这个\<\<是左移计算，改成二进制后逆运算，成功得到正确版本号。