# Introduction to Command Based Structures

This is just a series of notes written by William H. trying to learn how to get a command based robot working.
These notes are going to be totally informal and probably unorganized.

As opposed to a periodic structure, the command based structure relies on commands. Commands can be a bunch of things, but you can still have them function identically to a periodic framework if desired, but it has the flexibility to do things much more dynamically than a periodic framework (at least, it can do it much easier than a periodic can).

- [Introduction to Command Based Structures](#introduction-to-command-based-structures)
  - [CommandScheduler](#commandscheduler)
  - [Command Types](#command-types)
    - [Using the Command Abstract Class](#using-the-command-abstract-class)
    - [Using Subsystem Lambdas](#using-subsystem-lambdas)
      - [Note in Implementing a Subsystem](#note-in-implementing-a-subsystem)
  - [How to Actually Use a Command Based System](#how-to-actually-use-a-command-based-system)


## CommandScheduler
All commands and subsystems should end up going into the **CommandScheduler**. The **CommandScheduler** is what actually ends up running the code and commands within your robot's codebase. Anything that isn't handed over to the **CommandScheduler** *will not be run*.

## Command Types
There is two main ways to create a Command
- Extend the **Command** Abstract Class
- Using Lambdas, return basic Commands within methods in a **Subsystem**

Generally, you should use the **Command** Abstract Class if you're making something more complex, as it's more flexible, and the commands in **Subsystems** to help keep things organized within a particular subsystem. There's also nothing stopping you from using the Abstract Class within a **Subsytem**, but the Lambda method will help keep things concise and easier to read as long as you're not making something that requires the flexibility of the Abstract Class.

### Using the Command Abstract Class
See `ExampleCommand.java` for basic structure of an extending class.

While ExampleCommand requires a subsystem within it's constructor, this isn't necessarily true of all Commands as the Abstract Class does not. This simply shows that you can add requirements to the Command to help the CommandScheduler

The main point of using the Command Abstract Class is to have a unique function on initialization (when it's first ran in the CommandScheduler), when run each iteration, and on end. Using Lambdas, this isn't ordinarily possible (at least not easily and cleanly).

### Using Subsystem Lambdas
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
})
```

This creates a Command of the run functionality with whatever code you put in there. It's a little ugly, but it's simply how Lambdas work in Java, and it's much easier to read due to how concise it is compared to creating an entire class dedicated to a simple command.

#### Note in Implementing a Subsystem
Just to be clear, there is both a **Subsystem** Interface, and a **SubsystemBase** Abstract Class. While you can use just the **Subsystem** Interface, you should in most cases use the **SubsystemBase** Abstract Class instead. The **SubsystemBase** performs simple, but effective implementations of the basic requirements of a Subsystem, making sure you can't forget an essential part of your Subsystem's implementation in your codebase.

## How to Actually Use a Command Based System
With a basic understanding of the underlying structures, you now need to know how to actually interact with your **Commands** and **Subsystems**. First off, as everything needs to be fed into the **CommandScheduler**, you should try to keep everything instanced/object oriented rather than static.

If you're using the **SubsystemBase** in your subsystems, upon creating your object the constructor should automatically register your Subsystem with the **CommandScheduler**, and once the robot is enabled it should automatically begin running the default commands from this Subsystem.

With a **Command**, you'll need to register it manually as there is no set time to start running the command. You'll need to decide when the command needs to be registered with the **CommandScheduler** based on your needs, but when *do* decide to register it, you'll use the following line of code:
`CommandScheduler.getInstance().registerSubsystem(targetToBeRegistered);`

With all this, you should be prepared to start building a CommandBased system as long as you already prior experience to programming a robot. If you don't, make sure to always ask your peers and mentors for help, assistance, knowledge, and whatever else you can force out of them.