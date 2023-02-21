---
title: "[Java] The Art of Exception Catching: Tips and Tricks for Creating Your Own Exceptions in Java"
category: technical
tags:
  - java
toc: true
toc_sticky: true
---

## Try/catch
The simplest form of a try/catch statement in Java is:

```java
try {
    // code that may throw an exception
} catch (Exception e) {
    // code to handle the exception
}
```

In this form, the code that may throw an exception is enclosed within the try block. If an exception is thrown, it is caught by the catch block. The exception object is passed to the catch block as a parameter, and the code within the catch block is executed to handle the exception.

### Catch multiple exceptions
In Java, you can catch multiple exceptions using either a multi-catch block or separate catch blocks for each exception.

1. Multi-catch block:
A multi-catch block allows you to catch multiple exceptions in a single catch block. This can help simplify your code and reduce duplication. To use a multi-catch block, list the exceptions you want to catch, separated by pipes (|), inside a single catch block.

Here's an example:

```java
try {
    // code that may throw multiple exceptions
} catch (IOException | SQLException | NullPointerException e) {
    // handle the exceptions
}
```

In this example, the catch block will handle IOException, SQLException, and NullPointerException.

2. Separate catch blocks:
You can also use separate catch blocks to handle each exception individually. This can be useful if you need to handle each exception differently or if you want to provide more specific error messages for each exception.

Here's an example:

```java
try {
    // code that may throw multiple exceptions
} catch (IOException e) {
    // handle the IOException
} catch (SQLException e) {
    // handle the SQLException
} catch (NullPointerException e) {
    // handle the NullPointerException
}
```

In this example, the catch blocks will handle IOException, SQLException, and NullPointerException separately.

Note that if you use separate catch blocks, you should order them from the most specific to the most general exception. This is because if an exception is caught by a more general catch block, the more specific catch blocks will not be executed.

## Try-with-resources
The Try-with-Resources statement is a Java language feature introduced in Java 7 that simplifies the management of resources, such as file streams or database connections, by automatically closing them when they are no longer needed.

With the Try-with-Resources statement, you declare one or more resources that should be closed at the end of the statement. The resources must implement the AutoCloseable or Closeable interface. These interfaces contain a single method close(), which is called automatically at the end of the statement.

The syntax for the Try-with-Resources statement is as follows:

```java
try (Resource1 resource1 = new Resource1();
     Resource2 resource2 = new Resource2();
     ...) {
    // code that uses the resources
} catch (Exception e) {
    // exception handling code
}
```

The resources are declared inside the parentheses following the try keyword, separated by semicolons. The curly braces following the parentheses contain the code that uses the resources. The resources are closed automatically at the end of the statement, whether an exception occurs or not.

### Exception thrown by close() method
If an exception is thrown from the try block, and the resource object's `close()` method also throws an exception, then the exception from the try block takes priority, and the exception thrown by `close()` is added as a "suppressed" exception to the original exception.

You can access the suppressed exceptions using the `Throwable.getSuppressed()` method, which returns an array of Throwable objects that were suppressed. 

Here's an example of using getSuppressed() with try-with-resources:

```java
public class Example {
    public static void main(String[] args) {
        try (ResourceOne resourceOne = new ResourceOne(); ResourceTwo resourceTwo = new ResourceTwo()) {
            // Code that uses resourceOne and resourceTwo
            throw new Exception("An exception occurred");
        } catch (Exception e) {
            Throwable[] suppressedExceptions = e.getSuppressed();
            System.out.println("Number of suppressed exceptions: " + suppressedExceptions.length);
            for (Throwable suppressedException : suppressedExceptions) {
                System.out.println("Suppressed exception: " + suppressedException);
            }
        }
    }
}

class ResourceOne implements AutoCloseable {
    @Override
    public void close() throws Exception {
        throw new Exception("Exception in ResourceOne");
    }
}

class ResourceTwo implements AutoCloseable {
    @Override
    public void close() throws Exception {
        throw new Exception("Exception in ResourceTwo");
    }
}
```

The output of running this program will be:

```
Number of suppressed exceptions: 2
Suppressed exception: java.lang.Exception: Exception in ResourceOne
Suppressed exception: java.lang.Exception: Exception in ResourceTwo

```

## Try/finally
In Java, the pattern of try/finally is commonly used when you need to acquire and release a lock. This pattern ensures that the lock is always released, even if an exception is thrown while the lock is held.

Here's an example of how this pattern might be used:

```java
Lock lock = new ReentrantLock();
try {
    lock.lock();
    // Do some work while holding the lock
} finally {
    lock.unlock();
}
```

In this example, we first create a ReentrantLock and acquire the lock by calling lock() on it. Then, we do some work while holding the lock. Finally, we release the lock by calling unlock() in a finally block. This guarantees that the lock is always released, even if an exception is thrown while we're holding it.

In general, a finally block should be used only for cleaning up resources that were acquired in the corresponding try block, such as closing files or releasing locks. Any other behavior should be avoided to prevent unexpected issues and hard-to-debug problems.

Here are some specific things to avoid in a finally clause:

- Throwing exceptions: If a finally block throws an exception, any previous exceptions that were caught in the corresponding try block will be lost. This can make it difficult to debug the code and identify the root cause of the problem.

- Returning from the method: If a finally block returns from the method, any exceptions caught in the corresponding try block will be lost. This can lead to unexpected behavior and difficult-to-debug issues.

- Performing I/O or other potentially blocking operations: If a finally block performs an I/O operation or other potentially blocking operation, it could cause the program to hang or become unresponsive.

- Modifying variables that are used in the try or catch blocks: If a finally block modifies variables that are used in the try or catch blocks, it can change the behavior of the program and make it more difficult to understand and debug.

## Uncaught exceptions
In Java, a stack trace is a listing of the method calls that led up to the point where an exception was thrown. You will see the stack trace when an unhandled exception is thrown and the JVM prints the stack trace to the console or log file.

In Java, you can store the stack trace of an exception by using its getStackTrace() method, which returns an array of StackTraceElement objects. Each element of the array represents a frame on the call stack, with the most recent frame at index 0.

You can also store the stack trace in a String by calling the printStackTrace() method on the exception object and passing it a PrintWriter or a PrintStream object that writes to a StringWriter. Once you have the stack trace as a String, you can log it or include it in an error message, for example.

Here's an example code snippet that demonstrates how to store the stack trace of an exception in a String:

```java
try {
    // Some code that may throw an exception
} catch (Exception e) {
    StringWriter sw = new StringWriter();
    PrintWriter pw = new PrintWriter(sw);
    e.printStackTrace(pw);
    String stackTrace = sw.toString();
    // Do something with the stack trace
}
```

This code catches any exception that may be thrown by the code in the try block, then creates a StringWriter and a PrintWriter to capture the stack trace in a String. Finally, the stack trace is stored in the stackTrace variable, which you can use as needed.


### The StackWalker class
The `StackWalker` is a utility class introduced in Java 9 that provides a way to traverse and access the stack trace of the current thread, or any other thread, in a more efficient and flexible way compared to the traditional `Thread.getStackTrace()` method.

The StackWalker class allows you to traverse the stack trace from the bottom up, skipping over hidden frames, and access additional information about each stack frame, such as the source file name, line number, and class loader.

This can be useful in situations where you need to extract information from the stack trace programmatically, such as in logging frameworks, debugging tools, or profiling applications. It can also be useful in scenarios where you need to analyze the stack trace to diagnose a problem, such as in error reporting or troubleshooting.

Here is an example of how to use StackWalker to print the stack trace of the current thread:

```java
StackWalker walker = StackWalker.getInstance();
walker.forEach(System.out::println);
```

This will print out the stack trace of the current thread, starting from the current method and going up to the main method.


> Attribution: This blog post was written entirely by ChatGPT, an AI language model trained by OpenAI. The content has been generated based on its training data and may not always be entirely accurate or up to date. However, the AI language model has tried to provide helpful and informative content on learning Java.