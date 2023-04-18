---
title: Passing a list as argument to a vararg method
date: 2023-04-18 09:30:16
tags:
---


In Java, a vararg method is a method that can take a varying number of arguments of the same type. To pass a list as an argument to a vararg method, you can convert the list to an array and then pass the array as the vararg argument.

Here is an example:

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
    
In this example, the `doSomething` method takes a vararg argument of type `String`. We create a list of strings and add three elements to it. To pass this list as an argument to `doSomething`, we first convert the list to an array using the `toArray` method of `ArrayList` and then pass this array as the vararg argument to `doSomething`.

Note that we pass an empty array as an argument to the `toArray` method. This is because the size of the resulting array is determined by the size of the list, and we do not know this in advance. By passing an empty array, the `toArray` method creates a new array of the correct size.