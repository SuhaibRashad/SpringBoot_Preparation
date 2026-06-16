# IDE 


> **When you click Run in IntelliJ, IntelliJ does NOT usually run your application using `java -jar`.**

Instead, it runs it using **classpath mode**.

---

# What You Think Happens

Many beginners think IntelliJ does:

```bash
java -jar app.jar
```

But that's usually NOT what happens.

---

# What IntelliJ Actually Does

Suppose:

```java
package com.imran;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

and:

```gradle
dependencies {
    implementation 'org.apache.commons:commons-lang3:3.17.0'
}
```

When you press:

```text
▶ Run
```

IntelliJ roughly executes:

```bash
java -cp <many paths> com.imran.Main
```

Notice:

```bash
-cp
```

NOT

```bash
-jar
```

---

# Where Are The Dependencies?

Gradle downloaded:

```text
commons-lang3.jar
```

into cache.

Something like:

```text
~/.gradle/caches/
```

or

```text
C:\Users\<user>\.gradle\
```

IntelliJ knows where those JARs are.

So it builds a huge classpath:

```text
build/classes
commons-lang3.jar
mysql.jar
logback.jar
...
```

and passes all of them to JVM.

---

# Visual Flow

```text
Your Code
     +
Gradle Dependencies
     +
IntelliJ Classpath
     ↓
java -cp ...
     ↓
Application Runs
```

No Fat JAR needed.

---

# Example

Suppose:

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

IntelliJ already knows:

```text
Need commons-lang3.jar
```

So it launches:

```bash
java -cp
build/classes;
commons-lang3.jar
com.imran.Main
```

Application works.

---

# Then Why Does java -jar Fail?

Suppose you build:

```bash
gradlew build
```

and get:

```text
app.jar
```

Now you leave IntelliJ.

Run:

```bash
java -jar app.jar
```

JVM only sees:

```text
app.jar
```

It does NOT see:

```text
commons-lang3.jar
```

because IntelliJ is no longer helping.

Result:

```text
NoClassDefFoundError
```

---

# Think Of IntelliJ As A Smart Assistant

While developing:

```text
IntelliJ
   ↓
Finds Dependencies
   ↓
Builds Classpath
   ↓
Runs JVM
```

Everything works.

---

# Think Of Production Server

Production server doesn't have IntelliJ.

It only receives:

```text
app.jar
```

Now JVM asks:

```text
Where are the dependencies?
```

Nobody answers.

Application crashes.

That's why deployment artifacts often use Fat JARs.

---

# How Spring Boot Usually Works

During development:

```text
IntelliJ
   ↓
Run Main Class
```

No Fat JAR required.

---

For deployment:

```bash
gradlew bootJar
```

creates:

```text
app.jar
```

containing:

```text
Application
Spring
Tomcat
Jackson
Logging
Dependencies
```

Then:

```bash
java -jar app.jar
```

works on any machine with Java installed.

---

# What IntelliJ Run Configuration Contains

If you open:

```text
Run Configuration
```

you'll see something like:

```text
Main Class:
com.imran.Main

Use classpath of module:
app.main
```

That line:

```text
Use classpath of module
```

is the secret.

It tells IntelliJ:

```text
Collect all dependency JARs
Add them to JVM classpath
Run Main
```

---

# Interview Answer

**Q: Why does an application run successfully from IntelliJ even though the generated JAR is not a Fat JAR?**

**Answer:**

IntelliJ typically runs applications using `java -cp` and automatically constructs the classpath containing the application's compiled classes and all dependency JARs. Therefore, dependencies are available at runtime even though they are not packaged inside the generated JAR. When running the JAR using `java -jar`, those dependencies are not automatically included unless a Fat JAR or equivalent packaging mechanism is used.

---

# One-Line Notes Version

```text
IntelliJ usually runs applications using java -cp, not java -jar.

It automatically builds a classpath containing all project dependencies, which is why applications work even when the generated JAR is not a Fat JAR.
```

This is also why you'll often hear senior developers say:

> "Works in IntelliJ but fails after deployment."

Usually the root cause is a classpath/dependency packaging issue.
