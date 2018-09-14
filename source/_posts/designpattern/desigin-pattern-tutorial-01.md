---
title: 设计模式 单例模式
date: 2018-03-20 14:11:18
tags: [java,设计模式]
categories: 设计模式
keywords: 设计模式 单例模式
---

### 单例模式
定义:是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例的特殊类。通过单例模式可以保证系统中，应用该模式的类一个类只有一个实例。即一个类只有一个对象实例.

适用场景:
1.资源共享的情况下，避免由于资源操作时导致的性能或损耗.如公共配置文件。
2.可以全局访问.
#### 饿汉式单例
```
public class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton(){
    }

    public static Singleton getInstance(){
        return instance;
    }
}
```
饿汉式单例在单例类被加载时候,就实例化了这个对象.

#### 懒汉式单例
```
public class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton(){
    }

    public static synchronized Singleton getInstance(){
        if(instance==null){
            instance = new Singleton();
        }
        return instance;
    }

```
懒汉式是在调用getInstance()取得实例方法的时候才会实例化这个对象.