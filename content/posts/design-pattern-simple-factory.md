---
title: 设计模式之简单工厂模式
date: 2018-11-01 10:10:49
tags: 
- 设计模式
- Java
categories:
- 设计模式
comments: true
---

## 一、定义
简单工厂模式(Simple Factory Pattern)：又称为静态工厂方法(Static Factory Method)模式，它属于类创建型模式。在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

<!--more-->

## 二、模式结构
简单工厂模式包含如下角色：

* VideoFactory: 工厂角色
工厂角色负责实现创建所有实例的内部逻辑

* Video:
抽象产品角色是所创建的所有对象的父类，负责描述所有实例所共有的公共接口

* PythonVideo/JavaVideo
具体产品角色是创建目标，所有创建的对象都充当这个角色的某个具体类的实例。

![](/images/design_pattern/simple_factory.png)



## 三、案例代码

```Java
public abstract class Video {
    public abstract void produce();
}
```

```Java
public class JavaVideo extends Video {
    public void produce() {
        System.out.println("Java 视频");
    }
}
```

```Java
public class PythonVideo extends Video {
    public void produce() {
        System.out.println("Python 视频");
    }
}
```

```Java
public class VideoFactory {
    public Video getVideo(String type) {
        if("java".equalsIgnoreCase(type)) {
            return new JavaVideo();
        } else if("python".equalsIgnoreCase(type)) {
            return new PythonVideo();
        } else {
            return null;
        }
    }
}
```

```Java
public class Test {
    public static void main(String[] args) {
        VideoFactory videoFactory = new VideoFactory();
        Video video = videoFactory.getVideo("java");
        if(video == null) {
            return;
        }
        video.produce();
    }
}
```

## 四、优缺点
优点：
1. 只需要传入一个正确的参数，就可以获取所需要的对象而无需知道起创建细节。

缺点：
1. 工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，违背开闭原则。

适用场景
1. 工厂类负责创建的对象较少；
2. 客户端只知道传入工厂类的参数对于如何创建对象(逻辑)不关心。