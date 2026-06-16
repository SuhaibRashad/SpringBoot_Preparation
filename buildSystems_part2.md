
## Why Do We Create JAR Files?

After writing Java code:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

Java compiles it into:

```text
Main.class
```

Instead of distributing many `.class` files, Java packages them into:

```text
JAR = Java Archive
```

Example:

```text
app.jar
```

A JAR is similar to a ZIP file that contains Java classes and resources.

---

# Normal JAR

A normal JAR contains only your application's classes.

Example:

```text
app.jar

└── Main.class
```

Run:

```bash
java -jar app.jar
```

This works only if there are no external dependencies.

---

# The Dependency Problem

Suppose your code uses:

```java
import org.apache.commons.lang3.StringUtils;
```

Now your application depends on:

```text
commons-lang3.jar
```

When Gradle builds:

```bash
gradlew build
```

Your JAR usually contains:

```text
Main.class
```

but not:

```text
StringUtils.class
```

Therefore:

```bash
java -jar app.jar
```

may fail with:

```text
NoClassDefFoundError
ClassNotFoundException
```

because dependency classes are missing.

---

# Running Without Fat JAR

You must provide dependencies manually.

Windows:

```bash
java -cp "app.jar;commons-lang3.jar" com.imran.Main
```

Linux/Mac:

```bash
java -cp "app.jar:commons-lang3.jar" com.imran.Main
```

Here:

```text
-cp = classpath
```

Classpath tells JVM where to search for classes.

---

# Skinny JAR

Contains only your code.

```text
app.jar

└── Main.class
```

Characteristics:

✔ Small size

✘ No dependencies

✘ Need separate libraries

Think:

"Only my code"

---

# Thin JAR

Contains:

```text
My Code
+
Dependency Information
```

Example:

```text
app.jar

 ├── Main.class
 └── Dependency Metadata
```

Important:

Thin JAR does NOT contain actual library classes.

It only contains information like:

```text
Needs:
- Spring
- Jackson
- MySQL Driver
```

---

# Does Thin JAR Download Every Time?

No.

First Run:

```text
Check cache
↓
Not Found
↓
Download
↓
Store locally
```

Second Run:

```text
Check cache
↓
Found
↓
Use local copy
```

Think:

Like installing an app once and using it many times.

---

# Hollow JAR

Contains runtime components but not your application.

Example:

```text
hollow.jar

 ├── Spring Classes
 ├── Tomcat Classes
 ├── Jackson Classes
 └── Runtime Infrastructure
```

Missing:

```text
Main.class
Business Logic
Controllers
```

Think:

"A server waiting for an application"

---

# Fat JAR / Uber JAR

Contains everything.

```text
app.jar

 ├── My Code
 ├── Spring
 ├── Jackson
 ├── MySQL Driver
 ├── Logback
 └── Embedded Tomcat
```

Run:

```bash
java -jar app.jar
```

No extra libraries required.

Think:

"Everything packed into one suitcase"

---

# Why Fat JAR Exists

Without Fat JAR:

```text
app.jar
spring.jar
jackson.jar
mysql.jar
logback.jar
```

Need to deploy all files.

If one is missing:

```text
ClassNotFoundException
```

Application fails.

Fat JAR solves this by bundling everything together.

Result:

```text
app.jar
```

Single deployable file.

---

# Normal JAR vs Fat JAR

| Feature             | Normal JAR | Fat JAR |
| ------------------- | ---------- | ------- |
| Application Classes | Yes        | Yes     |
| Dependencies        | No         | Yes     |
| Single File         | Usually No | Yes     |
| Easy Deployment     | No         | Yes     |
| Size                | Small      | Large   |


# Important Java Ecosystem Terms

## Jackson

Purpose:

Convert Java Objects ↔ JSON

Example:

Java Object:

```java
Student s = new Student("Imran",20);
```

JSON:

```json
{
  "name":"Imran",
  "age":20
}
```

Jackson performs this conversion automatically.

Think:

"JSON Translator"

---

# MySQL Driver

Java cannot directly communicate with MySQL.

A driver acts as a translator.

```text
Java Application
      ↓
 MySQL Driver
      ↓
 MySQL Database
```

Common Driver:

```text
mysql-connector-j.jar
```

Think:

"Database Translator"

---

# Logback

Logging Framework.

Instead of:

```java
System.out.println("Started");
```

Professional applications use:

```java
log.info("Started");
log.error("Database Failed");
```

Logback can write logs to:

```text
Console
Files
Database
Remote Server
```

Think:

"Professional Printing System"

---

# Tomcat

A Java Web Server.

Example:

User opens:

```text
http://localhost:8080/users
```

Request Flow:

```text
Browser
   ↓
Tomcat
   ↓
Spring Application
```

Think:

"Receptionist of a Web Application"

---

# Embedded Tomcat

Old Style:

```text
Install Java
Install Tomcat
Deploy Application
```

Modern Style:

```text
Application
+
Tomcat
=
One JAR
```

Now:

```bash
java -jar app.jar
```

starts both application and Tomcat.

Think:

"Tomcat already inside the JAR"

---

# WAR File

WAR = Web Archive

Used to deploy web applications.

Example:

```text
myapp.war
```

Old Deployment:

```text
Tomcat Installed
      ↓
Deploy myapp.war
      ↓
Tomcat Runs It
```

Modern Spring Boot often uses:

```text
myapp.jar
```

instead.

---

# Spring Classes

When you add Spring dependency:

```gradle
implementation "org.springframework.boot:spring-boot-starter-web"
```

Thousands of classes are downloaded.

Examples:

```text
DispatcherServlet.class
ApplicationContext.class
BeanFactory.class
```

These are Spring Classes.

---

# Runtime Infrastructure

Everything required while application runs.

Examples:

```text
Tomcat
Spring
Logging
Connection Pools
Database Drivers
```

Think:

House Infrastructure:

```text
Electricity
Water
Internet
```

Application Infrastructure:

```text
Tomcat
Spring
Logging
Drivers
```

---

# Dependency Metadata

Dependency Metadata = Information about dependencies.

Example:

```text
Need:
- Spring
- Jackson
- MySQL Driver
```

Actual libraries are NOT present.

Only information about them.

Used mainly in Thin JAR approaches.

---

# Java Servers

## Tomcat

Lightweight Web Server.

Most popular.

---

## WildFly

Enterprise Application Server.

Provides more features.

---

## JBoss

Older name.

WildFly evolved from JBoss.

---

## WebLogic

Enterprise Application Server from Oracle.

Common in large enterprises and banks.

---

# Frequently Asked Interview Questions

Q: What is a JAR?

Answer:
A JAR (Java Archive) is a compressed package containing Java classes and resources.

---

Q: What is a Fat JAR?

Answer:
A Fat JAR bundles application classes, dependencies, and runtime components into a single executable JAR.

---

Q: Why do we need Fat JAR?

Answer:
To simplify deployment by packaging all required libraries into one file.

---

Q: What is the difference between a Normal JAR and Fat JAR?

Answer:

Normal JAR:

* Only application classes

Fat JAR:

* Application classes
* Dependencies
* Runtime components

---

Q: What is a Thin JAR?

Answer:
A Thin JAR contains application classes and dependency information but not the actual dependency libraries.

---

Q: Does a Thin JAR download libraries every time?

Answer:
No. Libraries are downloaded once and stored in local cache. Future runs use the cached copies.

---

Q: What is Embedded Tomcat?

Answer:
A Tomcat server packaged inside the application JAR, allowing the application to run without installing Tomcat separately.

---

Q: What is Jackson?

Answer:
A library used to convert Java objects to JSON and JSON to Java objects.

---

Q: What is a MySQL Driver?

Answer:
A library that allows Java applications to communicate with MySQL databases.

---

Q: What is Logback?

Answer:
A logging framework used to generate and manage application logs.

---

Q: What is a WAR File?

Answer:
A Web Archive used for deploying web applications to application servers such as Tomcat.

---

Q: What is Classpath?

Answer:
Classpath is the list of locations where JVM searches for classes and libraries during execution.


# Part 3 - Creating a Fat JAR Using Gradle

## Scenario

Suppose we have:

```java
import org.apache.commons.lang3.StringUtils;

public class Main {
    public static void main(String[] args) {
        System.out.println(
            StringUtils.capitalize("imran")
        );
    }
}
```

Dependency:

```gradle
dependencies {
    implementation 'org.apache.commons:commons-lang3:3.17.0'
}
```

---

# What Happens During Build?

Run:

```bash
gradlew build
```

Gradle compiles:

```text
Main.java
    ↓
Main.class
```

and creates:

```text
build/libs/app.jar
```

But this JAR normally contains only:

```text
app.jar

└── Main.class
```

It does NOT contain:

```text
StringUtils.class
```

because that class belongs to:

```text
commons-lang3.jar
```

---

# What Happens When We Run?

Command:

```bash
java -jar app.jar
```

JVM starts.

Main class loads successfully.

Then JVM encounters:

```java
StringUtils.capitalize(...)
```

Now JVM searches for:

```text
org/apache/commons/lang3/StringUtils.class
```

inside:

```text
app.jar
```

Not found.

Result:

```text
Exception in thread "main"

java.lang.NoClassDefFoundError:
org/apache/commons/lang3/StringUtils
```

or

```text
java.lang.ClassNotFoundException
```

depending on the situation.

---

# Why Does This Error Occur?

Because:

```text
Main.class exists

StringUtils.class does not exist
```

inside the JAR.

The application knows the class name.

The JVM cannot find the actual class file.

---

# Solution 1 - Provide Dependencies Manually

Example:

```bash
java -cp "app.jar;commons-lang3.jar" Main
```

or Linux:

```bash
java -cp "app.jar:commons-lang3.jar" Main
```

Now JVM searches:

```text
app.jar
commons-lang3.jar
```

and finds StringUtils.

Application works.

---

# Solution 2 - Create a Fat JAR

Instead of carrying:

```text
app.jar
commons-lang3.jar
logback.jar
mysql.jar
```

merge everything into:

```text
fat-app.jar
```

Then:

```bash
java -jar fat-app.jar
```

works without additional libraries.

---

# Gradle Fat JAR Configuration

## Complete Configuration

```gradle
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.apache.commons:commons-lang3:3.17.0'
}

jar {

    duplicatesStrategy =
        DuplicatesStrategy.EXCLUDE

    manifest {
        attributes(
            'Main-Class': 'Main'
        )
    }

    from {
        configurations.runtimeClasspath.collect {
            it.isDirectory()
                ? it
                : zipTree(it)
        }
    }
}
```

---

# Line-by-Line Explanation

## plugins

```gradle
plugins {
    id 'java'
}
```

Enables Java support.

Provides tasks:

```text
compileJava
jar
build
test
```

---

## repositories

```gradle
repositories {
    mavenCentral()
}
```

Tells Gradle:

```text
Download dependencies
from Maven Central
```

---

## dependencies

```gradle
dependencies {
    implementation
        'org.apache.commons:commons-lang3:3.17.0'
}
```

Downloads commons-lang3 library.

---

## jar

```gradle
jar {
```

Customizes the JAR creation process.

---

## duplicatesStrategy

```gradle
duplicatesStrategy =
    DuplicatesStrategy.EXCLUDE
```

Many JARs contain:

```text
META-INF
LICENSE
MANIFEST.MF
```

Without this setting:

```text
Duplicate Entry Error
```

may occur.

---

## manifest

```gradle
manifest {
```

Adds metadata into:

```text
META-INF/MANIFEST.MF
```

---

## Main-Class

```gradle
attributes(
   'Main-Class': 'Main'
)
```

Tells JVM:

```text
Start execution here
```

Without this:

```bash
java -jar app.jar
```

fails with:

```text
no main manifest attribute
```

---

## from

```gradle
from {
```

Adds extra files into the generated JAR.

---

## runtimeClasspath

```gradle
configurations.runtimeClasspath
```

Contains:

```text
commons-lang3.jar
mysql.jar
logback.jar
spring.jar
```

all runtime dependencies.

---

## collect

```gradle
collect {
```

Iterates through every dependency.

---

## isDirectory()

```gradle
it.isDirectory()
```

Checks whether item is:

```text
Directory
or
JAR File
```

---

## zipTree()

```gradle
zipTree(it)
```

Opens a JAR as if it were a ZIP file.

Example:

```text
commons-lang3.jar

    ↓

StringUtils.class
RandomUtils.class
...
```

Gradle extracts these classes.

---

# What Does zipTree Actually Achieve?

Without:

```gradle
zipTree(it)
```

Fat JAR would contain:

```text
app.jar

 ├── Main.class
 └── commons-lang3.jar
```

JVM won't automatically use the nested JAR.

With:

```gradle
zipTree(it)
```

Gradle extracts contents:

```text
app.jar

 ├── Main.class
 ├── StringUtils.class
 ├── RandomUtils.class
 └── ...
```

Now JVM can directly load dependency classes.

This is the core mechanism behind a manually built Fat JAR.

---

# Important Interview Question

Q: Why does `java -jar app.jar` fail even though Gradle downloaded the dependency?

Answer:

Because Gradle downloads dependencies into its cache, but the generated JAR contains only the application's classes. The dependency classes are not packaged into the JAR unless a Fat JAR configuration or plugin is used.

---

# Important Interview Question

Q: What is the purpose of `zipTree()`?

Answer:

`zipTree()` treats dependency JARs as ZIP archives and extracts their contents into the final JAR, allowing all dependency classes to be packaged together and creating a Fat JAR.


---
---


# some more clarity 

---
About 

```
duplicatesStrategy = DuplicatesStrategy.EXCLUDE
```
---

# What is the Problem?

When creating a Fat JAR, Gradle does:

```gradle
from {
    configurations.runtimeClasspath.collect {
        zipTree(it)
    }
}
```

Remember:

```text
zipTree()
```

opens every dependency JAR and copies all files into your final JAR.

Suppose you have:

```text
commons-lang3.jar
logback.jar
mysql.jar
```

Gradle extracts all of them.

---

# What Does a JAR Actually Contain?

Most beginners think a JAR contains only `.class` files.

Reality:

```text
commons-lang3.jar

├── StringUtils.class
├── LICENSE
├── META-INF/
└── MANIFEST.MF
```

Another JAR:

```text
mysql.jar

├── Driver.class
├── LICENSE
├── META-INF/
└── MANIFEST.MF
```

Another JAR:

```text
logback.jar

├── Logger.class
├── LICENSE
├── META-INF/
└── MANIFEST.MF
```

Notice something?

All three contain:

```text
LICENSE
META-INF
MANIFEST.MF
```

with the SAME file names.

---

# What Happens During Fat JAR Creation?

Gradle starts copying.

First:

```text
commons-lang3.jar

LICENSE
```

copied.

Then:

```text
mysql.jar

LICENSE
```

Gradle says:

```text
Wait...

LICENSE already exists.
```

Now there are two files wanting the same location:

```text
LICENSE
```

inside the final JAR.

---

# The Conflict

Imagine copying files into a folder.

Folder already contains:

```text
LICENSE
```

and you try to copy another:

```text
LICENSE
```

Question:

```text
Should I keep first?
Should I replace it?
Should I fail?
```

Gradle needs instructions.

---

# Without duplicatesStrategy

Gradle may fail with errors like:

```text
Entry LICENSE is duplicate
```

or

```text
Duplicate entry META-INF/MANIFEST.MF
```

Build stops.

No JAR created.

---

# What EXCLUDE Means

```gradle
duplicatesStrategy =
    DuplicatesStrategy.EXCLUDE
```

means:

```text
If duplicate file exists,
keep first one,
ignore the rest.
```

Example:

```text
commons-lang3.jar
    LICENSE

mysql.jar
    LICENSE
```

Result:

```text
fat-app.jar

LICENSE
```

Only one survives.

The second one is ignored.

---

# Visual Example

Before merge:

```text
Jar A

LICENSE
MANIFEST.MF
A.class


Jar B

LICENSE
MANIFEST.MF
B.class
```

---

After merge with EXCLUDE:

```text
Fat Jar

LICENSE
MANIFEST.MF
A.class
B.class
```

Duplicate files removed.

---

# Why Is It Usually Safe?

Because:

```text
LICENSE
README
MANIFEST.MF
```

are usually metadata files.

Your application doesn't execute them.

The important files are:

```text
.class files
```

which usually have unique package names.

Example:

```text
org/apache/commons/StringUtils.class

com/mysql/Driver.class
```

No conflict.

---

# Real Interview Answer

**What is the purpose of `duplicatesStrategy = DuplicatesStrategy.EXCLUDE`?**

> During Fat JAR creation, multiple dependency JARs may contain files with the same path, such as `META-INF/MANIFEST.MF`, `LICENSE`, or `README`. `DuplicatesStrategy.EXCLUDE` instructs Gradle to keep the first occurrence and ignore subsequent duplicates, preventing duplicate-entry build errors.

---

# One-Line Notes Version

```text
duplicatesStrategy = DuplicatesStrategy.EXCLUDE

While merging dependency JARs into a Fat JAR, if multiple JARs contain files with the same name/path, Gradle keeps the first copy and ignores the duplicates to avoid duplicate-entry build errors.
```

The next thing you should understand deeply is **`manifest { attributes('Main-Class': 'Main') }`**, because many developers use it without actually knowing what `MANIFEST.MF` looks like inside a JAR. That concept connects directly to how `java -jar` works.


---


---
About 

```
manifest { attributes('Main-Class': 'Main') }
```
---

# First Understand the Problem

Suppose you have:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

You package it into:

```text
app.jar
```

Now you run:

```bash
java -jar app.jar
```

Question:

```text
How does JVM know which class contains main()?
```

A JAR can contain hundreds of classes.

Example:

```text
app.jar

├── Main.class
├── Student.class
├── Employee.class
├── Database.class
└── Utility.class
```

Which one should JVM start with?

It has no idea.

Therefore it needs instructions.

---

# Enter MANIFEST.MF

Every JAR contains a special file:

```text
META-INF/MANIFEST.MF
```

Think of it as:

```text
Instruction Sheet for JVM
```

or

```text
Configuration File of the JAR
```

---

# Example Manifest

```text
META-INF/MANIFEST.MF

Manifest-Version: 1.0
Main-Class: Main
```

The important line is:

```text
Main-Class: Main
```

This tells JVM:

```text
Start execution from Main class
```

---

# What Happens When You Run?

Command:

```bash
java -jar app.jar
```

JVM internally does:

### Step 1

Open JAR.

```text
app.jar
```

---

### Step 2

Look for:

```text
META-INF/MANIFEST.MF
```

---

### Step 3

Read:

```text
Main-Class: Main
```

---

### Step 4

Load:

```java
Main.class
```

---

### Step 5

Execute:

```java
public static void main(String[] args)
```

Program starts.

---

# Without Main-Class

Manifest:

```text
Manifest-Version: 1.0
```

No Main-Class.

Now run:

```bash
java -jar app.jar
```

Error:

```text
no main manifest attribute, in app.jar
```

Meaning:

```text
I found the JAR.
I found the Manifest.

But you never told me
which class to start.
```

---

# What Gradle Configuration Does

You write:

```gradle
jar {
    manifest {
        attributes(
            'Main-Class': 'Main'
        )
    }
}
```

---

## manifest 

Means:

```text
Customize MANIFEST.MF
```

---

## attributes

Means:

```text
Add entries into manifest
```

---

## 'Main-Class': 'Main'

Means:

```text
Write this line:

Main-Class: Main
```

inside MANIFEST.MF.

---

# Generated Manifest

Gradle automatically creates:

```text
META-INF/MANIFEST.MF
```

containing:

```text
Manifest-Version: 1.0
Main-Class: Main
```

---

# If Package Exists

Suppose:

```java
package com.imran;

public class Main {
}
```

Then:

```gradle
manifest {
    attributes(
        'Main-Class': 'com.imran.Main'
    )
}
```

because JVM needs the fully qualified class name.

Generated:

```text
Main-Class: com.imran.Main
```

---

# Why Fully Qualified Name?

Because there may be multiple Main classes.

Example:

```text
com.imran.Main
com.company.Main
com.test.Main
```

Which one should JVM execute?

Need exact path.

Therefore:

```text
com.imran.Main
```

is used.

---

# What If Manifest Points To Wrong Class?

Example:

```gradle
manifest {
    attributes(
        'Main-Class': 'ABC'
    )
}
```

but:

```text
ABC.class
```

doesn't exist.

Run:

```bash
java -jar app.jar
```

Error:

```text
Could not find or load main class ABC
```

because JVM trusted the manifest and tried to load a class that isn't there.

---

# Creating Manifest Manually

You can create:

```text
manifest.txt

Main-Class: Main
```

Notice:

```text
Blank line at end is required
```

Then:

```bash
jar cfm app.jar manifest.txt Main.class
```

Meaning:

```text
c -> create
f -> jar file
m -> use custom manifest
```

---

# Most Important Understanding

Many beginners think:

```gradle
manifest {
    attributes(
        'Main-Class': 'Main'
    )
}
```

somehow runs the application.

It does NOT.

It only writes:

```text
Main-Class: Main
```

inside:

```text
META-INF/MANIFEST.MF
```

Later, when you execute:

```bash
java -jar app.jar
```

the JVM reads that file and decides which class to launch.

---

# Visual Flow

```text
Gradle Build
     │
     ▼
Creates MANIFEST.MF
     │
     ▼
Main-Class: com.imran.Main
     │
     ▼
Packaged into app.jar
     │
     ▼
java -jar app.jar
     │
     ▼
JVM opens MANIFEST.MF
     │
     ▼
Reads Main-Class
     │
     ▼
Loads Main.class
     │
     ▼
Calls main()
```

---

# Interview Answer

**What is the purpose of `manifest { attributes('Main-Class': 'com.imran.Main') }`?**

> It adds the `Main-Class` entry to the JAR's `MANIFEST.MF` file. When `java -jar` is executed, the JVM reads this entry to determine which class contains the application's entry point (`main()` method) and starts execution from that class.

---

# One-Line Notes Version

```text
manifest { attributes('Main-Class': 'com.imran.Main') }

Adds Main-Class information into META-INF/MANIFEST.MF so the JVM knows which class to execute when java -jar is used.
```

A subtle but important next concept is: **Why does `java -jar` ignore the `-cp` (classpath) option, while `java -cp` requires you to specify the main class manually?** That's directly related to how the manifest works.


---

# Q3 

Why does java -jar ignore the -cp (classpath) option, while java -cp requires you to specify the main class manually?
---

This is one of the most important JVM concepts.

Many developers memorize:

```bash
java -jar app.jar
```

and

```bash
java -cp app.jar Main
```

without understanding why they behave differently.

---

# Two Different JVM Modes

## Mode 1: JAR Mode

```bash
java -jar app.jar
```

## Mode 2: Classpath Mode

```bash
java -cp app.jar Main
```

These are two different startup mechanisms.

---

# Mode 1: java -jar

Command:

```bash
java -jar app.jar
```

JVM says:

```text
I will start from a JAR file.
```

---

### What JVM Does

Step 1:

```text
Open app.jar
```

Step 2:

```text
Find META-INF/MANIFEST.MF
```

Step 3:

```text
Read Main-Class
```

Example:

```text
Main-Class: com.imran.Main
```

Step 4:

```text
Load com.imran.Main
```

Step 5:

```text
Call main()
```

---

### Important Point

In this mode:

```text
Manifest is the boss.
```

The JVM trusts the JAR's manifest.

---

# Mode 2: java -cp

Command:

```bash
java -cp app.jar com.imran.Main
```

Now JVM says:

```text
Don't look in a manifest.

I will tell you exactly
which class to run.
```

Here:

```bash
com.imran.Main
```

is supplied explicitly.

So JVM does not need:

```text
Main-Class
```

inside a manifest.

---

# Example

Suppose:

```text
app.jar

├── Main.class
├── Student.class
└── Employee.class
```

No manifest.

This still works:

```bash
java -cp app.jar Main
```

because JVM already knows:

```text
Run Main
```

from the command line.

---

# Why Doesn't java -cp Need Main-Class?

Because YOU already provided:

```bash
java -cp app.jar Main
```

The class name is:

```text
Main
```

No need to ask the manifest.

---

# Why Does java -jar Need Main-Class?

Because:

```bash
java -jar app.jar
```

contains only:

```text
app.jar
```

No class name provided.

So JVM asks:

```text
Which class should I run?
```

Manifest answers:

```text
Main-Class: Main
```

---

# Why -jar Ignores -cp

This confuses many people.

Suppose:

```bash
java -cp abc.jar -jar app.jar
```

People think JVM will use:

```text
abc.jar
+
app.jar
```

Wrong.

When JVM sees:

```bash
-jar
```

it switches into JAR mode.

Now:

```text
Classpath from command line ignored.
```

The JAR becomes the primary source.

---

# Why JVM Does This

Imagine:

```bash
java -cp lib.jar -jar app.jar
```

Now JVM has two sources telling it what to do.

Source 1:

```text
Classpath says something.
```

Source 2:

```text
Manifest says something.
```

To avoid ambiguity:

```text
-jar wins.
```

The JVM follows the JAR's manifest configuration.

---

# Real Example

Suppose:

Manifest:

```text
Main-Class: com.imran.Main
```

Run:

```bash
java -jar app.jar
```

JVM executes:

```text
com.imran.Main
```

---

Now:

```bash
java -cp app.jar com.test.Main
```

JVM executes:

```text
com.test.Main
```

Manifest completely ignored.

---

# Visual Comparison

## java -jar

```text
app.jar
   │
   ▼
MANIFEST.MF
   │
   ▼
Main-Class
   │
   ▼
main()
```

---

## java -cp

```text
Classpath
   │
   ▼
User Specifies Class
   │
   ▼
main()
```

---

# Interview Question

### What is the difference between `java -jar` and `java -cp`?

**Answer:**

`java -jar` starts an application using the JAR's manifest file and reads the `Main-Class` attribute to determine the entry point.

`java -cp` uses the supplied classpath and requires the main class to be explicitly provided on the command line. The manifest's `Main-Class` is ignored.

---

# Interview Question

### Why does `java -jar` ignore `-cp`?

**Answer:**

When the JVM runs in JAR mode (`-jar`), it uses the JAR's manifest to determine the application's entry point and class loading behavior. To avoid conflicting startup instructions, the command-line classpath is ignored.

---

# One-Line Notes Version

```text
java -jar app.jar

Uses META-INF/MANIFEST.MF and Main-Class entry to start the application.
Classpath from command line is ignored.
```

```text
java -cp app.jar com.imran.Main

Uses the supplied classpath and explicitly provided main class.
Manifest Main-Class is ignored.
```

This is the exact reason why Fat JARs became popular: once everything is packaged inside a single executable JAR with a proper manifest, users only need:

```bash
java -jar app.jar
```

instead of managing long classpath commands with dozens of dependency JARs.
