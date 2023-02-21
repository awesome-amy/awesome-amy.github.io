---
title: "[Java] The Art of Exception Throwing: When, Why, and How to Do It"
category: technical
tags:
  - java
toc: true
toc_sticky: true
---

## Declare exceptions in the method's signature
In Java, if a method may throw a checked exception, you should declare that exception in the method's signature. This allows the calling code to handle the exception or propagate it up the call stack.

To declare a checked exception in a method's signature, you add the throws keyword followed by the name of the exception. For example, here's how you could declare that a method foo may throw an IOException:

```java
public void foo() throws IOException {
    // method implementation
}
```

If the method may throw multiple exceptions, you can list them all in the throws clause, separated by commas. For example:

```java
public void bar() throws IOException, SQLException {
    // method implementation
}
```

Note that you only need to declare checked exceptions in the method signature if the method may throw them. If a method only throws unchecked exceptions, you don't need to declare them in the method signature.

### Declarable exceptions for subclasses
if a superclass method declares one or more checked exceptions, any subclass that overrides the method can only declare the same exceptions or a subset of those exceptions. It cannot declare additional checked exceptions that are not declared by the superclass method.

The reason for this rule is that it ensures that the contract of the superclass method is maintained by the subclass. If a subclass could declare additional checked exceptions, it would break the contract of the superclass method and potentially cause problems for clients of the superclass.

For example, suppose a superclass method doSomething() declares a checked exception IOException. If a subclass overrides this method and declares an additional checked exception SQLException, it would violate the exception specification rule. If a client of the superclass were to call the overridden method, they would not be aware of the additional exception and would not be able to handle it appropriately, potentially causing errors or unexpected behavior.

## Document with Javadoc @throws
The @throws tag in Javadoc is used to document the exceptions that a method may throw. It is typically used to document checked exceptions, but can also be used to document unchecked exceptions.

To use the @throws tag, you should include it in the Javadoc documentation for the method, followed by the name of the exception class that may be thrown. You can also provide a description of the circumstances under which the exception may be thrown.

Here's an example of how to use the @throws tag in Javadoc:

```java
/**
 * This method does something important.
 * 
 * @param arg1 the first argument
 * @param arg2 the second argument
 * @throws IOException if there is an I/O error
 * @throws IllegalArgumentException if the arguments are invalid
 */
public void doSomething(int arg1, String arg2) throws IOException, IllegalArgumentException {
    // Implementation goes here
}
```

In this example, the @throws tag is used to document two possible exceptions that may be thrown by the method: IOException and IllegalArgumentException. The description for the IOException exception indicates that it may be thrown if there is an I/O error, while the description for the IllegalArgumentException exception indicates that it may be thrown if the arguments are invalid.

## Throw early, catch late
"Throw early, catch late" is a golden rule of exception handling that suggests that you should throw exceptions as early as possible and catch them as late as possible.

The idea behind this rule is to make your code more robust and easier to maintain. By throwing exceptions early, you can prevent problems from propagating through your code and causing further issues. For example, if a method relies on a certain input parameter to be valid, you should check that parameter at the beginning of the method and throw an exception if it's invalid. This prevents the method from continuing with bad data, potentially causing further errors down the line.

On the other hand, catching exceptions late can make your code more resilient and better able to recover from errors. By catching exceptions at a high level in your code, you can take appropriate action to recover from the error, such as retrying the operation or displaying an error message to the user. If you catch the exception too early, you may not have enough information to take appropriate action, or you may hide the underlying cause of the error.

By following this rule, you can create more robust and maintainable code that is better able to handle errors and recover from failures.

## Rethrow exceptions
Rethrowing an exception means throwing the same exception that was caught by an exception handler. You should rethrow an exception when you are not able to handle it completely in the catch block, and want to let the calling method or higher-level exception handler handle it.

Here are some scenarios when rethrowing an exception may be appropriate:

- If you are unable to handle the exception meaningfully in the catch block and want to log it or provide some diagnostic information before passing the exception to a higher-level handler.
- If you want to wrap the caught exception with another exception that provides additional information about the error or context of the problem.
- If you want to translate the exception to another type of exception that is more appropriate for the current context or abstraction level.

When rethrowing an exception, it is important to maintain the original stack trace so that you can trace the error back to its original source. To do this, you can pass the caught exception as the cause of the new exception that you are throwing. This can be done using the constructor of the new exception, as shown below:

```java
try {
    // some code that may throw an exception
} catch (IOException e) {
    throw new MyCustomException("An error occurred while doing something", e);
}
```

In this example, the original IOException that was caught is passed as the cause of the new MyCustomException. This allows you to see the original stack trace when debugging the exception.

It's important to be selective when rethrowing exceptions, and to avoid creating too many levels of exception nesting. Each level of nesting can add overhead and make it harder to debug problems. It's generally best to catch exceptions at the lowest possible level and to rethrow only when necessary to provide additional information or to pass the exception up to a higher-level handler.

### When checked exception is not allowed
If a checked exception occurs in a method that is not allowed to throw a checked exception, there are several approaches you can take to handle the situation.

One approach is to catch the checked exception and wrap it in an unchecked exception, such as RuntimeException, before rethrowing it. This approach allows the code to propagate the exception up the call stack without requiring the calling method to declare the checked exception in its throws clause.

For example, suppose you have a method doSomething() that is not allowed to throw a checked exception, but it calls a method doSomethingDangerous() that throws a checked exception:

```java
public void doSomething() {
    try {
        doSomethingDangerous();
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

### The initCause() method
When rethrowing an exception, it is possible to wrap the original exception inside a new exception to provide more contextual information about the error. In such cases, the initCause() method can be used to associate the original exception with the new exception being thrown.

The initCause() method is typically used in a catch block when rethrowing an exception. After creating a new exception object, the initCause() method can be called on the new exception to associate the original exception as the cause.

Here's an example:

```java
public void someMethod() throws MyException {
    try {
        // some code that may throw an exception
    } catch (IOException e) {
        MyException newException = new MyException("An error occurred");
        newException.initCause(e);
        throw newException;
    }
}
```

In this example, the someMethod() throws a custom MyException which is not allowed to throw a checked exception. However, the code inside the method can encounter an IOException which is a checked exception. To handle this, the catch block catches the IOException and creates a new MyException object with the message "An error occurred". The initCause() method is called on the new exception to associate the original IOException as the cause of the new exception. Finally, the new exception is thrown.

By using initCause(), the original exception is preserved as the cause of the new exception, which can be useful for debugging and error handling.

### Exception in lambda expressions
In Java, any method that declares a checked exception must either handle the exception or declare it in its method signature using the throws keyword. When using lambda expressions with functional interfaces that declare checked exceptions, the same rule applies.

If the abstract method declared in the functional interface throws a checked exception, any lambda expression that is assigned to that functional interface must either catch the exception or declare it in its method signature using the throws keyword.

However, if the lambda expression throws a checked exception that is not declared in the functional interface, the code will not compile. This is because the lambda expression is essentially defining a new implementation of the abstract method, and the new implementation cannot throw an exception that is not declared in the original method.


> Attribution: This blog post was written entirely by ChatGPT, an AI language model trained by OpenAI. The content has been generated based on its training data and may not always be entirely accurate or up to date. However, the AI language model has tried to provide helpful and informative content on learning Java.

## Use API methods to throw exceptions
`Objects.requireNonNull` is a utility method in Java that allows you to check whether an object is null, and throw a NullPointerException with a specific error message if it is. The method takes two arguments: the object to check, and the error message to include in the exception if the object is null.

There are several similar methods to Objects.requireNonNull() in Java that are used to perform various kinds of argument validation:

- `Objects.requireNonNullElse(T obj, T defaultObj)`: This method checks whether the specified object is null, and returns either the object or the default object if the object is null.

- `Objects.requireNonNullElseGet(T obj, Supplier<? extends T> supplier)`: This method checks whether the specified object is null, and returns either the object or the result of the supplier if the object is null.

- `Objects.checkIndex(int index, int size)`: This method checks whether the specified index is within the bounds of the specified size, and throws an IndexOutOfBoundsException if it is not.

- `Objects.checkFromIndexSize(int fromIndex, int size, int length)`: This method checks whether the specified range is within the bounds of the specified length, and throws an IndexOutOfBoundsException if it is not.
