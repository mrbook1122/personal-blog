
# Java函数式接口

## 函数式接口的定义

Java函数式接口是指只有一个抽象方法的接口。JDK8新增了一个注解@FunctionalInterface，这个注解用来标识一个接口是一个函数式接口，如果一个接口标识了函数式接口，但是不满足函数式接口的定义，则会编译出错。

```java
// 编译出错
@FunctionalInterface
interface FunInterface {
    void fun1();
    void fun2();
}
```

函数式接口两点注意事项：

1. 函数式接口可以有默认方法。
2. 如果一个接口定义了一个抽象方法，该抽象方法覆盖了Object类中的公共方法，则该方法不会计数为函数式接口中的抽象方法，因为任何类都需要实现Object类中的public方法。

例如:

```java
// 编译正常
@FunctionalInterface
interface FunInterface {
    boolean equals(Object obj);
    void fun();
}
```

## 函数式接口的使用

函数式接口除了具有普通接口的功能之外，还可以通过**lambda表达式**、**方法引用**、**构造器引用**来创建实例。

### 方法引用

方法引用有三种：引用类方法、引用对象的实例方法、引用类的实例方法
