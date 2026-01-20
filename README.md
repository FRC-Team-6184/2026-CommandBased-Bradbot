# Introduction to Command Based Structures

This is just a series of notes written by William H. trying to learn how to get a command based robot working.
These notes are going to be totally informal and probably unorganized.

As opposed to a periodic structure, the command based structure relies on commands. Commands can be a bunch of things, but you can still have them function identically to a periodic if desired, but it has the flexibility 

## Command Types
There is two main ways to create a Command
- Extend the **Command** Abstract Class
- Using Lambdas, return basic Commands within methods in a **Subsystem**

Generally, you should use the **Command** Abstract Class if you're making something more complex, as it's more flexible, and the commands in **Subsystems** to help keep things organized within a particular subsystem. There's also nothing stopping you from using the Abstract Class within a **Subsytem**, but the Lambda method will help keep things concise and easier to read as long as you're not making something that requires the flexibility of the Abstract Class.

## Using the Command Abstract Class
See `ExampleCommand.java` for basic structure of an extending class.

While ExampleCommand requires a subsystem within it's constructor, this isn't necessarily true of all Commands as the Abstract Class does not. This simply shows that you can add requirements to the Command to help the CommandScheduler

The main point of using the Command Abstract Class is to have a unique function on initialization (when it's first ran in the CommandScheduler), when run each iteration, and on end. Using Lambdas, this isn't ordinarily possible (at least not easily and cleanly).

## Using Subsystem Lambdas
See `ExampleSubsystem.java`'s `exampleMethodCommand` for basic structure of a lambda.

The lambdas are a bit ugly, but their quick, easy, and much more concise than setting up an entire class for a command. 

There is a set of basic commands with simple functionality laid out within the **Subsystem** Interface. These are:
- idle
- run
- runOnce
- startEnd
- runEnd
- startRun

I won't be explaining every command here, look at `Subsystem.class` for documentation of these.

These commands define the expected behavior for the CommandScheduler, and it is your job as the programmer to define it's behavior using a lambda. A basic example follows:
```java
run(() -> {
    //Your code goes here
})```

This creates a Command of the run functionality with whatever code you put in there. It's a little ugly, but it's simply how Lambdas work in Java, and it's much easier to read due to how concise it is compared to creating an entire class dedicated to a simple command.