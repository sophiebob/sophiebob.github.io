---
title: 设计模式 工厂模式
date: 2018-03-20 14:11:18
tags: [java,设计模式]
categories: 设计模式
keywords: 设计模式 单例模式
---

工厂模式是 Java 中最常用的设计模式之一。我们在创建对象时不会对客户端暴露创建的逻辑，通过使用一个共同的类来指向新创建的对象。
工厂模式分为：简单工厂模式、工厂方法模式、抽象工厂模式

#### 简单工厂模式
定义：一个非常简单的类主要负责帮我们生产不同的产品。

简单工厂主要实现:产品接口、产品实现、工厂实现（工厂接口）。   
产品接口:把相同的行为抽象出来定义成接口，定义产品的规范。   
产品实现:实现产品接口，执行具体行为。  
工厂实现（工厂接口）:产品与调用者的交互。 
```
public interface ISender {
     void send();
}

public class MailSender implements ISender{

    @Override
    public void send() {
        System.out.println("发送邮件");
    }
}

public class SmsSender implements ISender{

    @Override
    public void send() {
        System.out.println("发送短信");
    }
}

public class SendFactory {
    public ISender sendMsg(String type) {
        if ("mail".equals(type)) {
            return new MailSender();
        } else if ("sms".equals(type)) {
            return new SmsSender();
        } else {
            System.out.println("不支持此类型");
            return null;
        }
    }
}

public class Test {
    public static void main(String[] args) {
        SendFactory factory = new SendFactory();
        ISender sender = factory.produce("sms");
        sender.send();
    }
}
```
在这个过程我们在 SendFactory 中创建对象，客户端只需告诉工厂类用短信还是email发送。  
如果此时我们需要用QQ发送短信，我们需要修改 SendFactory 中 sendMsg 方法。这样修改效率很低且很麻烦。

#### 工厂方法模式
如果想拓展程序必须对工厂类进行修改，违背了闭包原则。这里我们需要使用 `工厂方法模式`，创建一个工厂接口和创建多个工厂实现类，大大降低了耦合度。并且可以可以扩展工厂类。
```
public interface IFactory {
    ISender sendMsg();
}

public class SendMailFactory implements IFactory{

    @Override
    public ISender sendMsg() {
        return new MailSender();
    }
}

public class SendSmsFactory implements IFactory{

    @Override
    public ISender sendMsg() {
        return new SmsSender();
    }
}

public class Test {
    public static void main(String[] args) {
        IFactory factory = new SendMailFactory();
        ISender sender = factory.sendMsg();
        sender.send();
    }
}
```
这样如果我们需要用QQ发送消息，只需要实现QQSender、SendQQFactory。不需要修改原来的代码。工厂方法模式去掉了简单工厂模式中工厂方法的静态方法，使得它可以被子类继承。不同的产品可以创建新的工厂来实现。  
简单工厂模式跟工厂方法模式极其相似，简单工厂一般是通过静态方法创建产品，工厂方法模式通过工厂接口创建。所有简单工厂在扩展方面偏弱。

#### 抽象工厂模式
抽象工厂模式是所有形态的工厂模式中最为抽象和最具一般性的一种形态。抽象工厂模式是指当有多个抽象角色时，使用的一种工厂模式。抽象工厂模式可以向客户端提供一个接口，使客户端在不必指定产品的具体的情况下，创建多个产品族中的产品对象。根据里氏替换原则，任何接受父类型的地方，都应当能够接受子类型。因此，实际上系统所需要的，仅仅是类型与这些抽象产品角色相同的一些实例，而不是这些抽象产品的实例。换言之，也就是这些抽象产品的具体子类的实例。工厂类负责创建抽象产品的具体子类的实例。

```
public interface IShape {
    void draw();
}

public class Rectangle implements IShape {

    @Override
    public void draw() {
        System.out.println("画个长方形");
    }
}

public class Square implements IShape {

    @Override
    public void draw() {
        System.out.println("画个正方形");
    }
}

public interface IColor {
    void fill();
}

public class Blue implements IColor {

    @Override
    public void fill() {
        System.out.println("绿色");
    }
}

public class Red implements IColor {

    @Override
    public void fill() {
        System.out.println("红色");
    }
}

public interface IFactory {
     IColor getColor(String color);
     IShape getShape(String shape) ;
}


public class AFactory implements IFactory {

    @Override
    public IColor getColor(String color) {
        return new Red();
    }

    @Override
    public IShape getShape(String shape) {
        return new Square();
    }
}

public class BFactory implements IFactory {

    @Override
    public IColor getColor(String color) {
        return new Blue();
    }

    @Override
    public IShape getShape(String shape) {
        return new Rectangle();
    }
}

public class Test {

    public static void main(String[] args){
        IFactory factory = new AFactory();
        factory.getShape("").draw();
        factory.getColor("").fill();
    }
}

```
当创建的对象是一系列相互关联或相互依赖的产品时，便可以使用抽象工厂模式。如果网站主题更换。
简单工厂模式，工厂方法模式，抽象工厂模式他们都及其相似，主要目的都是为了解耦。

