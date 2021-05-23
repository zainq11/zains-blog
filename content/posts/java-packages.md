
---
title: "Not only JARs: Packaging java differently"
date: 2021-03-14T21:44:08-08:00
draft: false
---


## Rough notes

> A Module is a group of closely related packages and resources along with a new module descriptor file.
>> In other words, it's a “package of Java Packages” abstraction that allows us to make our code even more reusable.
>> Each module is responsible for its resources, like media or configuration files.

Module are higher up organizations constructs of packages and resources.

### jlink

jlink is a tool that generates a custom Java runtime image that contains only the platform modules that are required for a given application. https://openjdk.java.net/jeps/220


The traditional method of creating a Java artifact has been a JAR (Java Archive) file format. While this still remains the more prevalant way of doing it, recent Java enhancements have introduced alternatives. These alternatives seem to be created with the intent of being able to create artifacts in formats that are more ubiquitous with the platform they run on. This gives end users a more natural installation experience
This means that we can install Java applications in formats such as `.exe` and `.msi` for Windows; `.deb` and `.rpm` for Linux; `pkg` and `dmg` for MacOS. One of the major benefits of this approach is that the end user does not need to perform additional steps of installing Java on the target machine.

The fact that we do not need Java to be installed separately on the target machine triggers some more questions. How the Java application image is actually created? What is responsible for transforming the byte code to native code? Well, in this case the java runtime is actually bundled into the image. So, when a user runs a Java program, they are actually triggering the baked in Java runtime to load the necessary java modules and invoke the necessary entrypoint. More on this later.

A valid follow up concern to this would be the additional overhead of baking in the JRE into the image. This could be the hundreds of MBs of overhead on size of the complete JRE.

Let us take a closer look at the two major Java enhancements that make this possible.

- ## jpackage

The goal of this enhancement is simple and that is to provide a way to package Java applications in a more self-contained way. This means that users can create installers that are native to the OS the application is intended to run on.
This along with `jlink` completes the end to end story to achieve this feat.

- ## Project Jigsaw (Java 9)

Let us flash back to the launch of Java 9. Java 9, the much awaited successor to the wildly popular Java 8, introduced the concept of [Java modules](https://openjdk.java.net/jeps/261). A Module is a group of closely related packages and resources along with a new module descriptor file called `module-info.java`. This allowed Java users to create modular java applications where the dependencies and exports of each module had to be laid out explicitly. To simplify, the module was essentially an independent package of packages.
Not only was this concept introduced to the users of Java, the JDK and JRE itself were modularized. For more details, please see [here](https://openjdk.java.net/jeps/220).

