# Part 2 — Why Spring Boot Exists

Before understanding Spring Boot, you must understand:

> Spring Boot was created because Spring itself became painful.

Many beginners hear:

```text
Spring → Old
Spring Boot → New
```

This is wrong.

Spring Boot is built **on top of Spring**.

Think:

```text
Spring = Engine

Spring Boot = Car built around the engine
```

The engine still exists.

---

# The Problem Spring Solved

Spring solved:

* Tight coupling
* Dependency Injection
* Bean management
* Object lifecycle

Developers loved it.

But a new problem appeared.

---

# The New Problem

Building a Spring application required lots of configuration.

Before Spring Boot, creating a web application could require:

```text
20+
30+
50+
configuration files and settings
```

Developers kept writing the same configuration repeatedly.

---

# What Is Configuration?

Stop and define.

Configuration means:

> Instructions telling a framework how to behave.

Example:

```java
Database URL
Port Number
Username
Password
```

These are not business logic.

They are settings.

Those settings are configuration.

---

## Real World Analogy

Imagine buying a new phone.

Before using it, you configure:

* Language
* WiFi
* Time zone
* Wallpaper

Those settings are configuration.

Same idea.

---

## Problem It Solves

Customizing framework behavior.

## Why It Exists

Applications need different settings.

## Interview Answer

Configuration is metadata used to customize framework behavior.

## One-Line Note

Configuration = settings for the application.

---

# Spring Before Boot

To create a web application:

You needed:

```text
Spring
Spring MVC
Tomcat
XML Configuration
Servlet Configuration
Dependency Configuration
Bean Configuration
```

Let's define those.

---

# What Is Spring MVC?

MVC stands for:

```text
Model
View
Controller
```

It is Spring's web framework.

Used for:

```text
Browser Request
        ↓
Controller
        ↓
Business Logic
        ↓
Response
```

Example:

```java
@GetMapping("/hello")
public String hello() {
    return "Hello";
}
```

Spring MVC makes web APIs possible.

---

## Problem It Solves

Handling HTTP requests.

## Why It Exists

Web applications need request-response handling.

## Interview Answer

Spring MVC is Spring's web framework implementing MVC architecture.

## One-Line Note

Spring MVC = web layer of Spring.

---

# What Is Tomcat?

Tomcat is a:

> Web Server and Servlet Container

New concept.

Let's define both.

---

# What Is A Web Server?

A web server:

* Receives requests
* Sends responses

Example:

```text
Browser
   |
   | GET /hello
   v
Server
```

Server responds:

```text
Hello
```

---

## Real World Analogy

Restaurant waiter.

Customer:

```text
Can I get a pizza?
```

Waiter:

```text
Here is your pizza.
```

Waiter = Server.

---

# What Is A Servlet?

A Servlet is:

> Java class that handles web requests.

Example:

```java
public class HelloServlet
       extends HttpServlet {

}
```

Before Spring MVC people wrote lots of Servlets.

---

# What Is Servlet Container?

A servlet container:

* Creates servlets
* Runs servlets
* Manages servlet lifecycle

Tomcat is one.

---

## Problem It Solves

Running Java web applications.

## Why It Exists

Servlets need an environment.

## Interview Answer

Tomcat is a servlet container and web server.

## One-Line Note

Tomcat runs Java web applications.

---

# Building Spring Application Before Boot

Typical project:

```text
src
 ├─ controllers
 ├─ services
 ├─ repositories

web.xml
dispatcher-servlet.xml
applicationContext.xml
pom.xml
```

Lots of files.

---

# What Is XML?

XML = Extensible Markup Language.

Example:

```xml
<bean
 id="userService"
 class="com.demo.UserService"/>
```

Used for configuration.

---

## Why Developers Hated It

Simple class:

```java
class UserService {}
```

Needed configuration:

```xml
<bean
 id="userService"
 class="com.demo.UserService"/>
```

For hundreds of classes:

```text
Hundreds of XML entries
```

Very repetitive.

---

# Example: Registering A Bean

Without Spring Boot:

```xml
<bean
 id="userRepository"
 class="com.demo.UserRepository"/>
```

```xml
<bean
 id="userService"
 class="com.demo.UserService"/>
```

```xml
<bean
 id="userController"
 class="com.demo.UserController"/>
```

Imagine doing this for:

```text
500 classes
```

Painful.

---

# Problem #1 Spring Had

Too much XML.

---

# Problem #2 Dependency Management

New concept.

---

# What Is Dependency Management?

Remember dependency (library) now means:

> External code your project uses.

Example:

```text
Spring
Hibernate
Jackson
JUnit
```

These are libraries.

---

# What Is A Library?

A library is:

> Pre-written code you can reuse.

Example:

Instead of writing:

```java
JSON parser
```

yourself,

you use:

```text
Jackson
```

library.

---

## Real World Analogy

Instead of making bricks yourself:

Buy bricks.

Library = ready-made bricks.

---

# Problem Before Boot

To build a web application:

You had to manually find versions.

Example:

```xml
spring-core 5.x
spring-web 5.x
spring-context 5.x
spring-mvc 5.x
```

Question:

```text
Which versions are compatible?
```

Developers constantly fought version issues.

---

# Common Error

```text
ClassNotFoundException
```

or

```text
NoSuchMethodError
```

because libraries didn't match.

---

# Example

You use:

```text
Spring Core 5.0
Spring MVC 4.0
```

Not compatible.

Application fails.

---

# Problem #2

Dependency version hell.

---

# Problem #3 Server Setup

Before Boot:

You needed Tomcat separately.

Install:

```text
Tomcat
```

Build:

```text
WAR file
```

Deploy:

```text
Copy WAR to Tomcat
```

Start Tomcat.

Then application runs.

---

# What Is A WAR?

WAR means:

```text
Web Application Archive
```

It is similar to:

```text
JAR
```

but for web applications.

---

# Traditional Flow

```text
Write Code
    ↓
Build WAR
    ↓
Install Tomcat
    ↓
Copy WAR
    ↓
Start Tomcat
    ↓
Application Runs
```

---

# Why This Was Annoying

For every developer:

```text
Install Tomcat
Configure Tomcat
Deploy WAR
Restart Tomcat
```

Again and again.

---

# Problem #4 Boilerplate Configuration

What Is Boilerplate?

> Repetitive code/configuration everyone writes.

Example:

Every web application needs:

```text
Dispatcher Servlet
Component Scan
View Resolver
MVC Configuration
```

Developers kept writing same thing.

---

# Real World Analogy

Imagine every new car requires manually installing:

```text
Engine
Wheels
Battery
Seats
```

before driving.

People want:

```text
Buy car
Drive car
```

Not assemble it.

---

# Spring Team Asked

A very important question:

```text
What configurations do developers write
in almost every project?
```

Answer:

Mostly the same.

Then they asked:

```text
Why not configure them automatically?
```

That idea became:

# Spring Boot

---

# Spring Boot's Main Goal

Not:

```text
Replace Spring
```

Instead:

```text
Make Spring easy
```

---

# Definition

Spring Boot is:

> An opinionated framework built on top of Spring that provides sensible defaults and automatic configuration.

New concept.

Let's define.

---

# What Is Opinionated?

Opinionated means:

> Framework already chooses common defaults.

Example:

Instead of asking:

```text
Which server?
```

Boot says:

```text
Tomcat
```

Instead of asking:

```text
Which JSON library?
```

Boot says:

```text
Jackson
```

Instead of asking:

```text
Which logging framework?
```

Boot says:

```text
Logback
```

You can still change them.

But Boot provides defaults.

---

## Real World Analogy

Restaurant menu.

Instead of:

```text
Choose bread
Choose cheese
Choose sauce
Choose vegetables
```

Boot gives:

```text
Standard Burger
```

Ready immediately.

Customize later if needed.

---

## Problem It Solves

Decision overload.

## Why It Exists

Most applications use similar choices.

## Interview Answer

Opinionated means Spring Boot provides sensible defaults.

## One-Line Note

Opinionated = defaults already chosen.

---

# Spring Boot Philosophy

```text
Convention Over Configuration
```

New concept.

---

# What Does It Mean?

Instead of:

```text
Tell framework everything
```

Boot assumes common conventions.

Example:

Boot sees:

```java
@RestController
```

and automatically configures web infrastructure.

You don't manually configure everything.

---

## Real World Analogy

Airport security.

You follow standard process.

No need to explain every step.

Convention exists.

---

## Problem It Solves

Too much configuration.

## Why It Exists

Most projects follow similar patterns.

## Interview Answer

Convention over Configuration means framework assumes common defaults instead of requiring explicit configuration.

## One-Line Note

Convention > Configuration.

---

# Big Picture

Without Boot

```text
Developer
   |
   +--> Configure Spring
   +--> Configure MVC
   +--> Configure Tomcat
   +--> Configure Beans
   +--> Configure Libraries
   +--> Configure Scanning
```

With Boot

```text
Developer
    |
    +--> Add Dependency
    +--> Write Code
```

Huge simplification.

---

# ASCII Flow

Before Boot

```text
Create Project
      ↓
Add Spring Dependencies
      ↓
Configure XML
      ↓
Configure MVC
      ↓
Install Tomcat
      ↓
Create WAR
      ↓
Deploy WAR
      ↓
Run
```

After Boot

```text
Create Project
      ↓
Add Starter
      ↓
Run Main Method
      ↓
Application Running
```

---

# Interview Question

### Why was Spring Boot introduced?

**Answer**

```text
Spring solved dependency injection and bean management,
but configuring Spring applications required large amounts
of XML, dependency management, and server setup.

Spring Boot was introduced to reduce configuration,
provide auto-configuration, starter dependencies,
embedded servers, and faster application development.
```

---

# Notes Version (Copy-Paste Friendly)

**Suggested filename:** `02-Why-Spring-Boot-Exists.md`

```text
WHY SPRING BOOT EXISTS

Problems In Traditional Spring

1. Too much XML
2. Dependency version management
3. External server setup
4. Boilerplate configuration

Configuration
- Settings for framework

Spring MVC
- Web framework of Spring

Tomcat
- Web server + servlet container

Servlet
- Java class handling web requests

WAR
- Web Application Archive

Opinionated Framework
- Provides sensible defaults

Convention Over Configuration
- Framework assumes common setup

Spring Boot Goal
- Make Spring easier
- Reduce configuration
- Faster development

Without Boot
- XML
- WAR deployment
- External Tomcat
- Manual dependency versions

With Boot
- Starter dependencies
- Embedded server
- Auto configuration
- Run main method

Spring Boot != Spring Replacement

Spring Boot = Spring + Automation + Defaults
```

# Current Mental Model

```text
Spring solves:
   Object management

Spring Boot solves:
   Spring configuration complexity
```