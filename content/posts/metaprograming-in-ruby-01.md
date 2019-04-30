---
title: Ruby元编程读书笔记01
date: 2017-12-24 09:42:13
categories:
- Ruby
tags:
- Ruby
- 元编程
comments: true
---

## 什么是元编程？
元编程: 编写能在运行时操作语言构件的代码.

```ruby
class Movie < ActiveRecord::Base
end

movie= Movie.create
movie.title = "Zootropolis"
move.title
```

<!--more-->

## 打开类
打开类：可以重新打开已经存在的类并对之进行动态修改.

```ruby
require "monetize"
price = Monetize.from_numeric(99, "USD")
price.format # => "$99.00"

```
Numeric是一个标准的Ruby类，我们可以利用打开类定义一个to_money方法，而不重新修改Numeric标准类的源码,具体操作是：

```ruby
class Numeric
  def to_money(currency = nil)
    Money.from_numeric(self, currency || Money.default_currency)
  end
end
require "monetize"
price = 99.to_money
price.format # => "$99.00"
```
打开类不能乱用，容易曹成猴子补丁问题

## 类的真相
一个对象的实例变量存在于对象本身之中，而已对象的方法存在于对象自身的类中。于是同一个类的对象共享同样的方法，但不共享实例变量。

当代码包含到别的代码中时使用模块。

当代码被实例化或者被继承时使用类。

Ruby的模块和类就像是目录，而常量则像是文件。于是可以像目录组织文件一样，用模块来组织常量。只是用来充当常量容易的模块，我们称之为命名空间。