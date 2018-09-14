---
title: synchronized 和 static synchronized
date: 2018-03-01 15:33:57
tags: synchronized
categories: java
keywords: synchronized
---

&emsp;&emsp;synchronized代表这个方法加锁,相当于不管哪一个线程（例如线程A），运行到这个方法时,都要检查有没有其它线程B（或者C、 D等）正在用这个方法(或者该类的其他同步方法)，有的话要等正在使用synchronized方法的线程B（或者C 、D）运行完这个方法后再运行此线程A,没有的话,锁定调用者,然后直接运行。它包括两种用法：synchronized 方法和 synchronized 块。

### synchronized 和 static synchronized 区别
`synchronized` 对类的当前实例（即当前对象）进行加锁（对象锁）。可以使用 `synchronized` 来标记一个方法或者代码块。当某个线程调用该对象的synchronized方法或者代码块时，该实例也就有一个监视块，防止线程并发访问该实例的synchronized保护块，其他线程暂时无法访问这个方法或者代码块，只能等待这个方法执行完毕或者代码块执行完毕才能访问。

`static synchronized` 对类进行加锁（类锁）。可以使用 `synchronized` 来标记一个方法。当某个线程调用该方法时，其他线程（包括不同实例）暂时无法访问这个方法。

### 举例
```
public class Something {

    public Object lock = new Object();
    public static Object staticLock = new Object();

    public synchronized void isSyncA() {}

    public synchronized void isSyncB() {}

    public static synchronized void cSyncA() {}

    public static synchronized void cSyncB() {}

    public void lockSyncA() {
        synchronized (lock) {
        }
    }

    public void lockSyncB() {
        synchronized (lock) {
        }
    }


    public void lockStaticSyncA() {
        synchronized (staticLock) {
        }
    }

    public void lockStaticSyncB() {
        synchronized (staticLock) {

        }
    }

}

```

假如有Something类的两个实例x与y，那么下列各组方法被多线程同时访问的情况是怎样的？

a. x.isSyncA()与x.isSyncB()   
b. x.isSyncA()与y.isSyncA()  
c. x.cSyncA()与y.cSyncB()  
d. x.isSyncA()与Something.cSyncA()  
c. x.lockSyncA()与y.lockSyncB()
d. x.lockStaticSyncA()与y.lockStaticSyncB()

> a. 同一个实例（x）访问isSyncA()与x.isSyncB()，所有不能同时访问，需要等待。
> b. 不同实例(x,y)访问，所以可以同时访问。
> c. `static synchronized`修饰方法。无论是否同一实例都不能同时访问，需要等待。
> d. synchronzied的是实例方法与synchronzied的类方法由于锁定（lock）不同的原因,所有可以访问。
> c. lock是两个对象，不同对象的引用，所以可以同时访问。
> d. staticLock为静态对象，即同一对象的引用，所以不可以同时访问，需要等待。

 synchronized(this) 和 方法上加synchronized作用一样。