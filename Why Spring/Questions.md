1. Core Architecture & Problem-Solving
Why does the Spring Framework exist? What problems does it solve?

Expected focus: Moving away from monolithic configurations, manual object instantiation, and hard-coded dependencies toward loose coupling.

What is the difference between Tight Coupling and Loose Coupling? Can you give a code example?

Expected focus: In tight coupling, a class instantiates its dependencies internally using the new keyword (e.g., private Engine engine = new Engine();), making it refractory (stubborn/unmanageable) to change or swap implementations. Loose coupling relies on interfaces and having those dependencies provided from the outside.

2. Inversion of Control (IoC) & Dependency Injection (DI)
What is Inversion of Control (IoC), and how does Spring implement it?

Expected focus: Flipping the control of the application flow and object creation from the developer's manual code to the underlying framework.

Is Inversion of Control the same as Dependency Injection?

Expected focus: Clarifying that IoC is the broader design principle, while Dependency Injection (DI) is the specific design pattern Spring uses to achieve IoC.

How does Dependency Injection improve code testability?

Expected focus: Because dependencies are passed into a class rather than hardcoded inside it, you can easily pass mock objects (like a mock repository or mock database connection) during unit testing.

3. The Spring Container & Beans
What is a Spring Bean?

Expected focus: A Java object that is instantiated, assembled, and otherwise managed by the Spring IoC container. It is not just a plain Java object (POJO) created by you via new.

What are the primary responsibilities of the Spring Container?

Expected focus: Highlighting the core life cycle: reading configuration metadata, instantiating/creating the beans, wiring (injecting) dependencies together, storing them in context, and managing their destruction.


