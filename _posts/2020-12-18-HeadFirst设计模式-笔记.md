---
layout:     post
title:      "《Head First 设计模式》杂记"
# subtitle:   ""
date:       2020-12-18
author:     "Paul"
header-img: "img/post-bg-2020-12.jpg"
tags:
    - 技术
---
[[C++ Code Reference]](https://github.com/Paul-HIT/HeadFirst-DesignPatterns)

1. 设计原则
- 源自策略模式

    a. 多用组合，少用继承。

    b. 针对接口编程，不针对实现编程。

- 源自观察者模式

    a. 为交互对象之间的松耦合设计而努力。

- 源自装饰者模式

    a. 类应该对扩展开放，对修改关闭。

- 源自工厂模式

    a. 要依赖抽象，不要依赖具体类。

    b. 遵循依赖倒置原则。

2. 观察者模式一般用于处理一对多关系。

3. 遵循依赖倒置原则
- 当你直接实例化一个对象时，就是在依赖它的具体类。
- 不能让高层组件依赖底层组件；不管是高层组件还是底层组件，都应该依赖抽象不应该依赖具体类。
- 依赖倒置原则是一种好的设计。在依赖倒置原则中指的是和一般的 OO 设计的思考方式完全相反。正常的 OO 设计是高层组件依赖底层组件，如在 PizzaStore 类中会 New Pizza，这就相当于 PizzaStore 这种高层类在依赖底层类 Pizza。

4. 在项目中使用以下步骤来尽量避免违反“依赖倒置”原则：
- 变量不可以持有具体类的引用；
- 不要让类派生自具体类；
- 不要覆盖基类中已经实现的方法，因为该方法之所以在基类中被实现，就应该是被所有派生类通用的。

5. 工厂模式
- 工厂方法：使用一个在子类中“延迟”实现的方法来实现工厂。工厂方法模式定义类一个创建对象的接口，但由子类决定要实例化但类是哪一个。工厂方法将类的实例化延迟到子类。
- 抽象工厂：提供一个接口类，用于创建相关或依赖对象的家族，而不需要明确指定具体类。
- 工厂模式可以将对象的创建封装起来，以便于得到更松的耦合，更有弹性的设计。

6. 单例模式：确保一个类只有一个实例，并提供一个全局访问入口点。
- private 的构造函数
- 拥有一个静态成员变量：uniqueInstance
- 通过一个 public 静态方法 getInstance 来作为全局访问的入口。
```C++
class Singleton {
public:
    static Singleton getInstance() {
        if (m_uniqueInstance == nullptr) {
            m_uniqueInstance = new Singleton();
        }
        return m_uniqueInstance;
    }

    static void deleteInstance() {
        if (m_uniqueInstance != nullptr) {
            delete m_uniqueInstance;
            m_uniqueInstance = nullptr;
        }
    }

private:
    Singleton() {

    }
    ~Singleton() {

    }

    static Singleton* m_uniqueInstance;
};
```

7. 能够应付多线程的单例模式（线程安全）
    a. 如果 getInstance() 的性能对应用程序不是很关键，就将 getInstance 改成同步的方法；
    b. 使用“急切”的方式在加载这个类的时候就创建好对象，而不是在需要的时候“延迟”实例化；(在 C++ 中使用内部静态变量来实现)

```C++
class Singleton {
public:
    Singleton& getInstance() {
        static Singleton m_uniqueInstance;
        return m_uniqueInstance;
    }
private:
    Singleton() {

    }
    ～Singleton() {

    }
};
```

    c. 双重检查（Double Check）：使用同步区块，减少使用同步，以提升性能。
