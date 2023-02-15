---
title: "Java: Inheritance"
category: technical
tags:
  - java
toc: true
toc_sticky: true
---

> Attribution: This blog post was written entirely by ChatGPT, an AI language model trained by OpenAI. The content has been generated based on its training data and may not always be entirely accurate or up to date. However, the AI language model has tried to provide helpful and informative content on learning Java.

## Sealed class
Sealed classes is a new feature that was introduced in Java 15. A sealed class is a special type of class that restricts the number of possible subclasses that can extend it. Sealed classes are declared using the "sealed" keyword and the subclasses that are allowed to extend it are declared using the "permits" keyword.

A sealed class can be thought of as a way to define a closed hierarchy of related classes, where only a specific set of classes are allowed to be its subclasses. This helps to enforce encapsulation and maintain code stability, as it prevents other classes from extending the sealed class and potentially introducing bugs or security vulnerabilities.

To create a sealed class in Java, you first define the class using the "sealed" keyword, followed by the list of permitted subclasses using the "permits" keyword. For example:

```Java
sealed class Animal permits Dog, Cat, Bird {
   // class implementation
}
```

In this example, the "Animal" class is declared as a sealed class that permits only the "Dog", "Cat", and "Bird" subclasses to extend it. Any other attempt to create a new subclass of "Animal" outside of this permitted set will result in a compilation error.

Here are some examples of where sealed classes could be useful in a Java program:

* Authentication and Authorization - You could use sealed classes to create a closed hierarchy of user types, such as "Administrator", "Employee", and "Customer". Only specific classes would be allowed to extend the "User" sealed class, ensuring that the system remains secure and that sensitive information is only accessible to authorized users.

* Currency hierarchy - In a program that handles different currencies, you could use a sealed class to define a closed hierarchy of currencies, such as "USD", "EUR", and "GBP". Only specific subclasses would be allowed to extend the "Currency" sealed class, ensuring that the program is robust and that all currency conversions are handled accurately.

To create a valid subtype of a sealed type, the subtype must meet the following requirements:

1. The subtype must be declared in the same module as the sealed class, or in a module that exports its package to the module containing the sealed class.

2. The subtype must explicitly declare that it extends the sealed class, using the "extends" keyword followed by the name of the sealed class.

3. The subtype must be either final, sealed or non-sealed. A final subtype cannot have any further subclasses, while a sealed subtype can have further subclasses as long as they are also declared in the permitted set.

4. If the sealed class has any abstract methods, the subtype must implement all of them.

5. If the sealed class has any non-private constructors, the subtype must either implement all of them or define its own constructors that call a non-private constructor of the sealed class.

Overall, sealed classes in Java provide a way to enforce strict class hierarchies and improve the overall design and stability of your code.