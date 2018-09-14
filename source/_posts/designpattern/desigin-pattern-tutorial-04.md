---
title: 设计模式 外观模式
date: 2018-03-20 14:11:18
tags: [java,设计模式]
categories: 设计模式
keywords: 设计模式 外观模式
---

#### 外观模式
外观模式是隐藏系统的复杂性，向客户端提供一个简单的接口。降低访问复杂系统的内部子系统时的复杂度，简化客户端与之的接口。

比如：证件系统Card和用户系统User相互关联。客户端不直接和用户系统、证件系统交互，而是通过外观Manager交互。
```
public class Card {
    private String number;
    private int type;
    ...
}

public class User {
    private String name;
    private String sex;
    private int age;
    ...
}
    
public class Manager {

    private Card card;
    private User user;

    public void setInfo(String number, int type, String name, String sex, int age) {
        card.setNumber(number);
        card.setType(type);
        user.setName(name);
        user.setAge(age);
        user.setSex(sex);
    }
}    
```
主要减少系统相互依赖，但是修改代码比较麻烦。
更形象的例子：关闭暖风机总电源，暖风机加热功能和吹风功能都关闭了。