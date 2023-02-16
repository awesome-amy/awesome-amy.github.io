---
title: "[Java] Printing Java Objects like a Pro: The Power of toString()"
category: technical
tags:
  - java
toc: true
toc_sticky: true
---

## The java.lang.Object Class
In Java, `Object` is a class that is the root of the class hierarchy. All other classes in Java are subclasses of `Object`, either directly or indirectly.

The `Object` class defines several methods that are available to all Java classes. Some of the most commonly used methods of `Object` are:

* `toString()`: Returns a string representation of the object.
* `equals(Object obj)`: Indicates whether some other object is "equal to" this one.
* `hashCode()`: Returns a hash code value for the object.
* `getClass()`: Returns the runtime class of the object.

Because all Java classes are subclasses of `Object`, they inherit these methods and can override them as needed. Additionally, the `Object` class provides a few other useful methods, such as `wait()`, `notify()`, and `notifyAll()`, which are used for inter-thread communication.

In summary, `Object` is a fundamental class in Java that provides a base set of functionality for all other classes in the language.

## toString method
Overriding the `toString()` method to return a string representation of an object with the class name and square brackets is another common practice in Java. This format can be helpful because it provides more information about the object's class and can make the output more easily readable.

To override the `toString()` method with `getClass().getName()` and square brackets, you can use the following code as an example:

```java
public class MyClass {
    private String name;
    private int value;

    // constructors, getters, and setters here

    @Override
    public String toString() {
        return getClass().getName() + "[" + name + ", " + value + "]";
    }
}
```

In this example, the `toString()` method returns a string representation of the `MyClass` object with the class name and square brackets. The string includes the name of the class, obtained using the `getClass().getName()` method, followed by the values of the `name` and `value` instance variables, separated by a comma and a space.

If you create an instance of `MyClass` and call the `toString()` method, you will get a string representation of the object with the class name and values in square brackets, like this:

```java
MyClass obj = new MyClass("foo", 42);
System.out.println(obj.toString()); // output: com.example.MyClass[foo, 42]
```
