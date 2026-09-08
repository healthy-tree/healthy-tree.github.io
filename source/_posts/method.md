---
title: Java 实参妙用
date: 2023-04-18 09:30:16
tags: [Java, List]
categories: [SE]
description: 在 Java 中将 List 作为可变参数（vararg）方法的参数传递
---

在 Java 中，可变参数（vararg）方法是指可以接收数量可变的同类型参数的方法。要把一个 List 作为实参传给可变参数方法，可以先将 List 转换为数组，再把该数组作为可变参数传入。

下面是一个示例：

```java
public static void doSomething(String... args) {
    // code here
}

List<String> myList = new ArrayList<>();
    myList.add("arg1");
    myList.add("arg2");
    myList.add("arg3");

doSomething(myList.toArray(new String[0]));
```

在这个例子中，`doSomething` 方法接收一个 `String` 类型的可变参数。我们创建一个字符串列表并向其中添加了三个元素。要把这个列表作为实参传给 `doSomething`，我们首先使用 `ArrayList` 的 `toArray` 方法将列表转换为数组，再将该数组作为可变参数传给 `doSomething`。

注意，我们传给 `toArray` 方法的是一个空数组。这是因为结果数组的大小由列表的大小决定，而我们事先并不知道这个大小。通过传入一个空数组，`toArray` 方法会创建一个大小正确的新数组。
