Exceptions Best Practices
================================

This document is agnostic of programming language, but you should be familiar with the concept of exceptions in the language you're working with.

If you're not, good resources exist elsewhere, including for [Java](https://docs.oracle.com/javase/tutorial/essential/exceptions/) and [C#](https://msdn.microsoft.com/en-us/library/ms173160.aspx).

## Common Causes of Exceptions

- Failure to connect to services external to your application, such as a database or API.
- Failure to read/write from/to a file.
- Failed attempt to convert data, such as trying to convert something like `"dog"` to an integer. This happens most often when accepting user input.
- Null pointers. In other words, an object reference -- such as a method parameter or local variable -- that doesn't actually contain an object.

## Best Practices

## The Golden Rules of Handling Exceptions

From *Foundations of Programming* by Karl Seguin ([online version](http://openmymind.net/FoundationsOfProgramming.pdf)):

1. Only handle exceptions that you can actually do something about.
2. You can't do anything about the vast majority of exceptions.

For example, if your code receives an exception due to a database not being available, there's probably nothing your program can do about it.

However, if you receive invalid input from a user, you would be able to re-prompt them, along with an error message to help them get it right the next time.

## Which Types to Catch

When working with a try/catch statement, in statically-type languages you can declare the type of exception you wish to catch. Given inheritance and polymorphism, catching the base `Exception` type will result in *all* exceptions being caught. This is a bad thing to do.

How is it possible for you, as the programmer, to be able to anticipate every possible exception that might be thrown? And further, how could you reasonably be able to handle every type of exception? You probably can't. Be specific about the types of exceptions you want to catch, and your code will be much better.

Catching the base class `Exception` -- that is, all exceptions -- is sometimes referred to as "exception swallowing" since in most cases, these exceptions are simply absorbed and not re-thrown or logged. If your program has a bug, or reaches an undesirable state, you want to know about it! **Don't swallow exceptions.**

## Which Types to Throw

As with catching, be specific with which types of exceptions you throw. **Never throw an instance of the base `Exception` class.** If a built-in exception type works well based on it's documented intended use, then use it! However, if there isn't a built-in exception that's appropriate, or if it's possible to provide more helpful information by using a custom exception class, then do so.

## How to Avoid Exceptions

For some types of exceptions, there's little you can do. If a database goes down, it's down. However, many situations that result exceptions are avoidable.

### Validate User Input

Validate user input to ensure that it is of the type your code expects, and satisfies any other implicit constraints (such as numeric input falling within a certain range).

If you're working within a framework such as ASP.NET or Spring Boot, use the built-in validation capabilities to make this easier.

Perhaps the most important thing to keep in mind here is that you should **never assume that input given to your program is safe and valid**. This is the case even when you're providing browser-based validation. Clever (or malicious) users can bypass most forms of client-side validation.

### Check For `null` References

If your code depends on an input parameter not being `null` to work properly, and it's possible to gracefully handle the situation -- for example, by re-prompting the user -- then you should do so.

As with exceptions above, if there is no way to reasonably recover from a null pointer, then you shouldn't "swallow" it. Furthermore, it's generally a bad idea to catch a null pointer exception (`NullPointerException` in Java, `NullReferenceException` in C#). [Read more](http://stackoverflow.com/questions/5991745/java-null-reference-best-practice) on why this is the case.


## References

### C&#35;

- [Exceptions and Exception Handling](https://msdn.microsoft.com/en-us/library/ms173160.aspx)
- [Exception Hierarchy](https://msdn.microsoft.com/en-us/library/z4c5tckx.aspx)

### Java

- [Exceptions Tutorial](https://docs.oracle.com/javase/tutorial/essential/exceptions/)
- [Effective Java Exceptions](http://www.oracle.com/technetwork/articles/entarch/effective-exceptions-092345.html)
- [Class Exception](http://docs.oracle.com/javase/8/docs/api/java/lang/Exception.html)
