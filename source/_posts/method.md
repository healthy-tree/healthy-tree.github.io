---
title: Java 实参妙用
date: 2023-04-18 09:30:16
tags: [Java, List, 可变参数]
categories: [SE]
description: 在 Java 中将 List 作为可变参数（vararg）方法的实参传递：原理、写法与常见陷阱
---

## 背景

可变参数（variable arity parameter，简称 vararg）方法是指形如 `void doSomething(String... args)`、可以接收数量不确定的同类型实参的方法。

需要明确的是：**可变参数在编译后就是数组**。也就是说，`String... args` 与 `String[] args` 的方法签名完全等价，调用方实际传入的必须是一个 `String[]`。正因为这个约束，`List` 不能作为实参直接传给可变参数方法，必须先转换为数组。

## 方案

将一个 `List` 传给可变参数方法，标准做法是：

1. 通过 `Collection.toArray(T[])` 把 `List` 转换为对应类型的数组；
2. 将该数组作为可变参数的实参传入。

## 示例

```java
public static void doSomething(String... args) {
    for (String arg : args) {
        System.out.println(arg);
    }
}

List<String> myList = new ArrayList<>();
myList.add("arg1");
myList.add("arg2");
myList.add("arg3");

// List -> 数组 -> 可变参数
doSomething(myList.toArray(new String[0]));
```

示例中 `doSomething` 声明了一个 `String` 类型的可变参数。我们先构造包含三个元素的列表，再调用 `toArray(new String[0])` 得到 `String[]`，最后将其作为实参传入，效果等价于：

```java
doSomething("arg1", "arg2", "arg3");
```

## 关键点解析

### 为什么要传入一个空数组

`toArray(T[] a)` 的契约是：

- 若传入数组的长度**小于**集合元素个数，则忽略该数组，新建一个**同类型、长度恰好等于元素个数**的数组返回；
- 若长度**足够**，则直接复用传入的数组（多余位置置为 `null`）。

由于列表长度在运行前通常未知，传入 `new String[0]` 可以让方法内部直接分配一个大小正确的新数组，既避免了容量不足，也不必额外计算 `size()`。

一个常见的替代写法是 `myList.toArray(new String[myList.size()])`。它在语义上同样正确，但存在两个小问题：一是需要先清零再填充，二是若 `size()` 与实际元素个数不一致（如并发修改）会产生尾部的 `null` 元素。因此，**`toArray(new String[0])` 是更推荐的写法**；现代 HotSpot 虚拟机还会对该模式做专门优化，性能通常不逊于预分配大小的写法。

### 类型必须匹配

数组的元素类型必须与可变参数的组件类型一致，否则会在编译期报错：

```java
static void printInts(Integer... args) {}
static void printIntsPrimitive(int... args) {}

List<Integer> nums = List.of(1, 2, 3);

printInts(nums.toArray(new Integer[0]));          // 正确：Integer[] 匹配 Integer...
printIntsPrimitive(nums.toArray(new Integer[0])); // 编译错误：Integer[] 无法作为 int... 的实参
```

基本类型的可变参数（如 `int...`）需要的是 `int[]`，`Integer[]` 不会自动拆箱为 `int[]`。

### 其他注意事项

- **空列表**：`toArray(new String[0])` 会返回长度为 0 的数组，调用可变参数方法是安全的，方法内 `args.length == 0`。
- **元素为 `null`**：数组会原样保留 `null` 元素，方法内部若对元素做解引用需自行判空。
- **列表本身为 `null`**：会抛出 `NullPointerException`，建议在入口处校验。
- **重载歧义**：若同时存在 `doSomething()` 与 `doSomething(String...)`，无参调用会优先匹配前者；直接传 `null` 时需注意编译器可能匹配到数组版本。
- **无需担心副作用**：`toArray` 返回的是新数组，被调用方对数组元素的修改不会影响原 `List`。

## 更简洁的写法

如果只是为了构造一组参数，而不必先使用 `List`，可以直接用以下方式，省去转换步骤：

```java
doSomething("arg1", "arg2", "arg3");                 // 直接列举
doSomething(List.of("arg1", "arg2").toArray(new String[0]));  // List.of 是只读列表
```

## 小结

- 可变参数的本质就是数组，因此接受 `String...` 的方法实参必须是一个 `String[]`；
- 将 `List` 传入可变参数方法的统一写法是 `list.toArray(new String[0])`；
- 注意组件类型匹配（尤其是基本类型）、`null` 处理以及重载带来的匹配歧义。
