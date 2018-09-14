---
title: 设计模式 组合模式
date: 2018-03-20 14:11:18
tags: [java,设计模式]
categories: 设计模式
keywords: 设计模式 组合模式
---

#### 组合模式
组合模式是把一组相似的对象合并在一起。按照树形结构来组合对象按层次区分。高层模块调用简单、节点自由添加。

将对象组合成树形结构以表示“部分-整体”的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。掌握组合模式的重点是要理解清楚 “部分/整体” 还有 ”单个对象“ 与 "组合对象" 的含义。


举例：创建IEmployee抽象类（也可以使用接口）。创建单个对象AndroidEmployee、IOSEmployee实现IEmployee。创建嘴和对象ManagerEmployee。
相当于主管负责管理ios和android开发。

```
public abstract class IEmployee {
    public abstract void add(IEmployee c);
    public abstract void remove(IEmployee c);
    
    private String name;
    private int salary;
    ...
}

public class AndroidEmployee extends IEmployee{
    @Override
    public void add(IEmployee c) {
    }
    
    @Override
    public void remove(IEmployee c) {
    }
}

public class IOSEmployee extends IEmployee{
    @Override
    public void add(IEmployee c) {

    }
    
    @Override
    public void remove(IEmployee c) {

    }
}

public class ManagerEmployee extends IEmployee{

    public List<IEmployee> list = new ArrayList<IEmployee>();
    @Override
    public void add(IEmployee c) {

    }

    @Override
    public void remove(IEmployee c) {

    }
}

public class Test {

    public static void main(String[] args) {
        IEmployee iosEmployee = new IOSEmployee();
        iosEmployee.setName("叼叼");
        iosEmployee.setSalary(15000);

        IEmployee androidEmployee = new AndroidEmployee();
        androidEmployee.setName("波波");
        androidEmployee.setSalary(13000);

        ManagerEmployee managerEmployee = new ManagerEmployee();
        managerEmployee.setName("灰灰");
        managerEmployee.setSalary(25000);

        managerEmployee.add(iosEmployee);
        managerEmployee.add(androidEmployee);
    }
}
```
#### 使用场景
网站导航栏、文件和文件夹管理、树形菜单等。只要是要体现整体与局部有相似关系都可以考虑组合模式。


#### 组合模式优点
组合模式包含了单个对象和组合对象的层次结构。组合对象可以包含许多单个对象，而这个组合对象又可以被其他组合对象包含。
