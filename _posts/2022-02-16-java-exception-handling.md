---
title: "[Java] Exception-ally Speaking: Navigating Checked and Unchecked Exceptions in Java"
category: technical
tags:
  - java
toc: true
toc_sticky: true
---

## Throwable, Error, and Exception
In Java, Throwable is the root class of the exception hierarchy. Both Error and Exception are subclasses of Throwable.

The main difference between Error and Exception is that Error represents an unrecoverable condition that should not be caught or handled by a program, while Exception represents a recoverable condition that can be caught and handled by a program.

Errors are generally caused by the environment in which a program is running, such as running out of memory or a stack overflow, and they usually indicate a serious problem that cannot be corrected by the program. Examples of errors in Java include OutOfMemoryError, StackOverflowError, and LinkageError.

Exceptions, on the other hand, represent exceptional conditions that occur within a program, such as invalid input, network failures, or unexpected conditions, and they can be handled by a program. Exceptions are further divided into two categories: checked exceptions and unchecked exceptions.

### Checked and unchecked exceptions

Checked exceptions are exceptions that must be handled by a program, either by catching the exception or by declaring that the method throws the exception. Examples of checked exceptions in Java include IOException and SQLException.

Unchecked exceptions are exceptions that do not need to be handled explicitly by a program, and they are usually caused by programming errors, such as null pointer dereferences or out-of-bounds array access. Examples of unchecked exceptions in Java include NullPointerException and ArrayIndexOutOfBoundsException.

In general, unchecked exceptions are used for programming errors or exceptional conditions that are not recoverable, while checked exceptions are used for errors that are recoverable and for which the caller should be able to take corrective action. The decision of whether to use a checked or unchecked exception depends on the context in which the method is being used, and whether or not the error is recoverable.

### RuntimeException
In Java, RuntimeException is a subclass of Exception and represents a type of unchecked exception. RuntimeExceptions are exceptions that can be thrown by the JVM at runtime and do not need to be declared in a method's signature or handled by a program explicitly.

Some common examples of RuntimeExceptions include NullPointerException, ArrayIndexOutOfBoundsException, and IllegalArgumentException.

When a RuntimeException occurs, the JVM automatically propagates the exception up the call stack until it is caught by an appropriate catch block, or until it reaches the top level of the program where it is typically handled by a default exception handler that prints an error message to the console.

So, what should you do when a RuntimeException occurs in your Java program? Here are some best practices:

* Fix the underlying cause of the exception: If the exception is caused by a programming error, such as a null pointer dereference or an out-of-bounds array access, you should fix the code that caused the error.

* Handle the exception: If the exception is caused by a user error, such as invalid input, you should handle the exception by displaying an appropriate error message to the user and allowing them to correct their input.

* Avoid catching RuntimeExceptions unnecessarily: Since RuntimeExceptions are unchecked exceptions, they do not need to be caught or declared in a method's signature. However, if you choose to catch a RuntimeException, you should make sure that you are not catching it unnecessarily and that you are not hiding the underlying cause of the exception.

* Log the exception: If you catch a RuntimeException, you should log the exception to a log file or to the console, so that you can diagnose and fix the underlying cause of the exception.

## IOException
In Java, IOException is a subclass of Exception that represents an I/O error that occurs while reading from or writing to a file or stream. An IOException is a checked exception, which means that it must be either caught or declared in the method signature.

Some common subclasses of IOException include FileNotFoundException, SocketException, and EOFException.

When an IOException occurs, it typically means that a file or network resource is unavailable or that an I/O operation failed for some other reason. Here are some best practices for throwing and handling IOExceptions in Java:

* Throw IOException when an I/O operation fails: If an I/O operation, such as reading from a file or writing to a network socket, fails, you should throw an IOException with a meaningful error message that describes the cause of the failure.

* Catch and handle IOException: If a method can throw an IOException, you should catch the exception and handle it appropriately. For example, you may display an error message to the user or retry the operation after a delay.

* Close resources properly: When working with files or streams, it is important to close them properly to avoid resource leaks and potential IOExceptions. You should use the try-with-resources statement to ensure that resources are closed automatically, even if an exception occurs.

Here is an example of how to throw and handle an IOException in Java:

```java
public void readFile(String filename) {
    try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
        String line = reader.readLine();
        // process the line
    } catch (IOException e) {
        System.err.println("Error reading file: " + e.getMessage());
    }
}
```

In this example, the readFile method attempts to read from a file using a BufferedReader. If an IOException occurs, the method catches the exception and prints an error message to the console. The try-with-resources statement is used to ensure that the reader is closed properly, even if an exception occurs.

> Attribution: This blog post was written entirely by ChatGPT, an AI language model trained by OpenAI. The content has been generated based on its training data and may not always be entirely accurate or up to date. However, the AI language model has tried to provide helpful and informative content on learning Java.
