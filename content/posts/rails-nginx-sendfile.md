---
title: Rails Nginx 实现下载受限文件的优化
date: 2018-02-24 20:29:43
categories:
- Ruby
tags:
- Rails
- Nginx
comments: true
---
## 需求

在Web开发中经常遇到一些文件需要用户认证和授权才能够访问，实现这样的功能需要在App Server进行。当遇到大文件或者并发量增加的时候，容易造成性能问题。为什么会这样呢？让我们从操作系统层面来分析一下：

<!--more-->

![](/images/zero_copy_01.png)
当下载一个文件的时候，应用程序会进行系统调用。操作系统首先会检查，是不是最近访问过此文件，文件内容是否缓存在内核缓冲区，如果是，操作系统则直接根据read系统调用提供的buf地址，将内核缓冲区的内容拷贝到buf所指定的用户空间缓冲区中去。如果不是，操作系统则首先将磁盘上的数据拷贝的内核缓冲区，这一步目前主要依靠DMA来传输，然后再把内核缓冲区上的内容拷贝到用户缓冲区中。接下来，write系统调用再把用户缓冲区的内容拷贝到网络堆栈相关的内核缓冲区中，最后socket再把内核缓冲区的内容发送到网卡上。

综上一共进行了4次数据的拷贝和4次上下文切换。那么有没有方法减少数据的拷贝和上下文切换？从Linux Kernel 2.4版本开始新增了sendfile()来实现这些过程的简化。
```C
#include<sys/sendfile.h>
ssize_t sendfile(int out_fd, int in_fd, off_t *offset, size_t count);
```

![](/images/zero_copy_02.png)
系统调用sendfile()在代表输入文件的描述符in_fd和代表输出文件的描述符out_fd之间传送文件内容（字节）。描述符out_fd必须指向一个套接字，而in_fd指向的文件必须是可以mmap的。这些局限限制了sendfile的使用，使sendfile只能将数据从文件传递到套接字上，反之则不行。

使用sendfile不仅减少了数据拷贝的次数，还减少了上下文切换，数据传送始终只发生在kernel space。

那么我们该如何在实际应用中使用sendfile()呢？幸好Rails和Nginx给我们提供高级的抽象，我们只需要简单的配置一下就可食用。

## 实现和配置

#### 假设

有一个请求访问受限文件test.txt, 该文件放在/mnt/data/test.txt
```
GET /api/download/files/test.txt HTTP/1.1
```
#### Nginx 配置

```
    location /_send_file_accel/{
        internal;
        alias /mnt/data/;
    }

    location /api {
        ...

        proxy_set_header   X-Sendfile-Type X-Accel-Redirect;
        proxy_set_header   X-Accel-Mapping /mnt/data/=/_send_file_accel/;

        ...
    }

```

#### Rails 配置

只需要在production.rb的config文件中加入
```Ruby
config.action_dispatch.x_sendfile_header = 'X-Accel-Redirect'
```

#### Rails controller 中的代码

```Ruby
def show
  ...
  file = user.test_file # 假设文件名为test.txt
  path = File.join('/mnt/data', file)
  send_file(path, 
    disposition: 'attachment', 
    status: 200)
end
```
