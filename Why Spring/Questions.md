## 1. Traditional Spring vs. Spring Boot

* **What specific problems does Spring Boot solve that existed in traditional Spring framework applications?**
* *Expected focus:* Mentioning the reduction of **tedious** (tiresome/monotonous) XML configurations, eliminating manual dependency version mismatches, and removing the need to manually set up an external Tomcat server.


* **Is Spring Boot a replacement for the Spring Framework?**
* *Expected focus:* Absolutely not. Spring Boot is an extension of Spring. It is essentially: $\text{Spring Boot} = \text{Spring} + \text{Automation} + \text{Sensible Defaults}$. It uses Spring under the hood but wraps it in auto-configuration.



## 2. Core Spring Boot Mechanics

* **What does it mean when we say Spring Boot is an "Opinionated" framework?**
* *Expected focus:* It means Spring Boot makes default decisions ("opinions") for you. It assumes that if you add a web dependency, you probably want an embedded Tomcat server running on port 8080 with standard configurations, saving you from writing boilerplate code.


* **Explain the concept of "Convention over Configuration".**
* *Expected focus:* The framework assumes a standard setup layout and configuration by default. You only need to write configuration code when you want to deviate from that standard convention.


* **What are Starter Dependencies in Spring Boot, and how do they help with version management?**
* *Expected focus:* Starters (like `spring-boot-starter-web`) are aggregators of common dependencies. Instead of manually matching versions for Spring MVC, Jackson, and Tomcat, you import one starter, and Spring Boot's parent BOM (Bill of Materials) manages compatible versions automatically.



## 3. Web Architecture & Deployment

* **What is an embedded server, and how does it change how you deploy Java applications?**
* *Expected focus:* In traditional Spring, you had to build a **WAR** (Web Application Archive) file and manually install and configure an external Tomcat instance. Spring Boot embeds Tomcat directly inside the executable **JAR** file, allowing you to run the app instantly by just executing the `main` method.


* **Can you briefly explain the relationship between a Servlet, Tomcat, and Spring MVC?**
* *Expected focus:* A Servlet is the fundamental Java class that handles HTTP web requests. Tomcat is the web server/servlet container that hosts and runs these servlets. Spring MVC is the high-level framework built on top of servlets to make web development organized and efficient.



---

# Part 2

## 1. Traditional Spring vs. Spring Boot

* **What specific problems does Spring Boot solve that existed in traditional Spring framework applications?**
* *Expected focus:* Mentioning the reduction of **tedious** (tiresome/monotonous) XML configurations, eliminating manual dependency version mismatches, and removing the need to manually set up an external Tomcat server.


* **Is Spring Boot a replacement for the Spring Framework?**
* *Expected focus:* Absolutely not. Spring Boot is an extension of Spring. It is essentially: $\text{Spring Boot} = \text{Spring} + \text{Automation} + \text{Sensible Defaults}$. It uses Spring under the hood but wraps it in auto-configuration.



## 2. Core Spring Boot Mechanics

* **What does it mean when we say Spring Boot is an "Opinionated" framework?**
* *Expected focus:* It means Spring Boot makes default decisions ("opinions") for you. It assumes that if you add a web dependency, you probably want an embedded Tomcat server running on port 8080 with standard configurations, saving you from writing boilerplate code.


* **Explain the concept of "Convention over Configuration".**
* *Expected focus:* The framework assumes a standard setup layout and configuration by default. You only need to write configuration code when you want to deviate from that standard convention.


* **What are Starter Dependencies in Spring Boot, and how do they help with version management?**
* *Expected focus:* Starters (like `spring-boot-starter-web`) are aggregators of common dependencies. Instead of manually matching versions for Spring MVC, Jackson, and Tomcat, you import one starter, and Spring Boot's parent BOM (Bill of Materials) manages compatible versions automatically.



## 3. Web Architecture & Deployment

* **What is an embedded server, and how does it change how you deploy Java applications?**
* *Expected focus:* In traditional Spring, you had to build a **WAR** (Web Application Archive) file and manually install and configure an external Tomcat instance. Spring Boot embeds Tomcat directly inside the executable **JAR** file, allowing you to run the app instantly by just executing the `main` method.


* **Can you briefly explain the relationship between a Servlet, Tomcat, and Spring MVC?**
* *Expected focus:* A Servlet is the fundamental Java class that handles HTTP web requests. Tomcat is the web server/servlet container that hosts and runs these servlets. Spring MVC is the high-level framework built on top of servlets to make web development organized and efficient.


# Part 3

