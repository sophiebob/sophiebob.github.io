---
title: 设计模式 适配器模式
date: 2018-03-20 14:11:18
tags: [java,设计模式]
categories: 设计模式
keywords: 设计模式 适配器模式
---

#### 适配器模式

适配器模式是作为两个不兼容接口之间的桥梁。这种类型的设计模式属于结构型模式，它结合了两个独立接口的功能。简单说就是把一个类的接口转换成所期待的另一个接口，把两个无法一起工作的类联系在一起。
比如：XMLReader提供了一个读取小电影的功能。现在我们想用JSONRead读取小电影再解析小电影。我们可以用XMLReader提供的方法直接读取店店员。
```
public interface IXMLReader {
    InputStream readXMLData();
}

public class XMLReader implements IXMLReader {

    @Override
    public InputStream readXMLData() {
        InputStream is = null;
        try {
            is = new FileInputStream("E:/电影/高清电影");
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        return is;
    }
}


public interface IJSONReader {
    InputStream readJsonData();
}

public class JSONRead extends XMLReader implements IJSONReader{

    @Override
    public InputStream readJsonData() {
        return readXMLData();
    }
}

public class Test {
    public static void main(String[] args) {
        IJSONReader ijsonReader = new JSONRead();
        ijsonReader.readJsonData();
    }
}

```
适配器模式分为：类设计模式、对象适配器。类适配器是通过继承类适配者类实现的。
对象适配器是创建一个适配者对象，调用客户所需的方法。
```
public class JSONRead2 implements IJSONReader{

    private XMLReader xmlReader = new XMLReader();
    @Override
    public InputStream readJsonData() {
        return xmlReader.readXMLData();
    }
}
```
理论上来说还是把接口设计好。比用设配器模式简单、方便、好用。