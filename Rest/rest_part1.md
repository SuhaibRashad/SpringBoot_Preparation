# REST API Fundamentals — Part 2

Now that we understand:

```text
Custom Languages Everywhere
            ↓
HTTP Standardized Communication
            ↓
API Design Chaos Still Exists
            ↓
Need for REST
```

Let's answer:

> Who created REST and what exactly is REST?

---

# First: Who Created REST?

A person named Roy Fielding.

Around the late 1990s, he was working on the architecture of the World Wide Web.

He wasn't trying to create "REST APIs."

He was trying to answer a bigger question:

> Why does the web scale to millions and billions of requests?

Think about it.

Every day:

```text
Browser
   ↓
Google
   ↓
Amazon
   ↓
YouTube
   ↓
Wikipedia
```

Billions of interactions happen.

Yet the web survives.

Why?

What design decisions made that possible?

REST was his attempt to describe those successful design principles.

---

# Important Misunderstanding

Most beginners think:

```text
REST
   =
GET
POST
PUT
DELETE
```

Wrong.

Those are tools commonly used in REST.

REST itself is a collection of architectural constraints.

---

# What is a Constraint?

New Concept.

A constraint means:

> A rule that limits freedom in exchange for some benefit.

---

# Real World Example

Traffic Signals

Without constraints:

```text
Drive anywhere
Any speed
Any direction
```

Freedom = High

Accidents = High

---

With constraints:

```text
Red = Stop
Green = Go
```

Freedom = Lower

Safety = Higher

---

Notice:

We sacrificed freedom.

We gained predictability.

---

# Why Constraints Matter

Let's use your ambulance example.

Imagine no rules.

Company A:

```text
Ambulance = White
```

Company B:

```text
Ambulance = Black
```

Company C:

```text
Ambulance = Pink
```

Nobody knows what's urgent.

---

Now government says:

```text
Must have siren
Must have emergency lights
Must follow emergency markings
```

Less freedom.

More predictability.

---

REST does the same thing.

It introduces constraints.

Not because constraints are fun.

Because consistency scales.

---

# What Happens Without REST Constraints?

Imagine your company has:

```text
100 Developers
50 APIs
20 Teams
```

Without constraints:

Team A:

```http
POST /fetchUser
```

Team B:

```http
POST /getCustomer
```

Team C:

```http
GET /retrievePerson
```

Team D:

```http
POST /loadUserData
```

Every team invents its own style.

---

New employee joins.

They spend months learning:

```text
Team A rules
Team B rules
Team C rules
```

instead of building features.

---

# REST's Core Idea

REST says:

> Let's create some architectural rules so every API behaves in a predictable way.

---

# REST Constraint #1 — Client Server Separation

This is the most important REST constraint.

Let's understand why it exists.

---

# The Problem Before Separation

Imagine a restaurant.

Kitchen:

```text
Cooks food
```

Waiter:

```text
Talks to customers
```

---

Now imagine:

```text
Customer enters kitchen
Changes recipes
Touches ingredients
Talks to chef directly
```

Chaos.

Nobody knows responsibilities.

---

# Software Version

Before clear separation:

```text
Frontend
Backend
Database
```

all become tightly mixed.

---

Example:

Mobile App directly changes database.

Website directly changes database.

Desktop app directly changes database.

---

Diagram:

```text
Mobile App ----\
                \
Website -------- Database
                /
Desktop App ---/
```

Looks simple.

Actually dangerous.

---

# Problems

## Problem 1: Security

Every client now accesses database.

More attack surface.

---

## Problem 2: Database Changes Break Everyone

Suppose database changes:

Old:

```text
customer_name
```

New:

```text
full_name
```

Now:

```text
Website breaks
Mobile breaks
Desktop breaks
```

Everything breaks.

---

## Problem 3: Hard to Scale

Thousands of clients directly hit database.

Database becomes overloaded.

---

# REST Solution

Separate responsibilities.

---

Client:

```text
UI
Screens
Buttons
Forms
```

---

Server:

```text
Business Logic
Validation
Security
Rules
```

---

Database:

```text
Stores Data
```

---

Diagram:

```text
Client
   ↓
Server
   ↓
Database
```

---

# Restaurant Analogy

Customer:

```text
Client
```

---

Waiter:

```text
Server
```

---

Kitchen:

```text
Database + Business Logic
```

---

Customer never enters kitchen.

Everything goes through waiter.

---

Why?

Because waiter provides:

```text
Order validation
Coordination
Rules
```

---

Exactly what server does.

---

# Internal Working

Let's see what happens when opening Swiggy.

---

Step 1

User opens app.

```text
Client
```

---

Step 2

App requests restaurant list.

```http
GET /restaurants
```

---

Step 3

Server receives request.

Checks:

```text
Authentication
Permissions
Business Rules
```

---

Step 4

Server queries database.

```sql
SELECT * FROM restaurants
```

---

Step 5

Database returns data.

---

Step 6

Server formats response.

---

Step 7

Client displays restaurants.

---

Diagram:

```text
Client
   ↓
Server
   ↓
Database
   ↑
Server
   ↑
Client
```

---

# Why Was It Designed This Way?

Because the web needed:

### Independent Evolution

Frontend changes.

Backend continues working.

---

Backend changes.

Frontend continues working.

---

Database changes.

Clients don't care.

---

Everyone evolves independently.

---

# Real World Example

Instagram.

You update app UI.

```text
New buttons
New screens
New colors
```

Backend can remain same.

---

Backend team updates recommendation algorithm.

UI remains same.

---

This separation allows thousands of engineers to work simultaneously.

---

# What Breaks Without This Constraint?

Imagine mobile app directly queries database.

Now:

```text
Database schema changes
```

↓

```text
Millions of phones break
```

Disaster.

---

# Production Example

Companies like:

* [Netflix](https://www.netflix.com?utm_source=chatgpt.com)
* [Amazon](https://www.amazon.com?utm_source=chatgpt.com)
* [Swiggy](https://www.swiggy.com?utm_source=chatgpt.com)

have many clients:

```text
Android
iPhone
Web
TV
Tablet
```

All communicate through APIs.

None directly touch databases.

---

# Tradeoffs

## Benefits

* Better security
* Better scalability
* Independent development
* Easier maintenance

---

## Costs

* Extra network calls
* More components
* More infrastructure

---

Nothing is free.

Every architectural decision has a cost.

---

# Common Beginner Mistake

Thinking:

```text
Client
   ↓
Database
```

is simpler.

It is simpler initially.

It becomes a nightmare at scale.

---

# Senior Engineer Perspective

A senior engineer sees:

```text
Client-Server Separation
```

and immediately thinks:

* Security boundaries
* Scalability
* Versioning
* Maintainability
* Team independence

Not just architecture diagrams.

---

# What You Must Understand From This Part

### Key Idea 1

REST is a collection of architectural constraints.

---

### Key Idea 2

Constraints reduce freedom but improve consistency.

---

### Key Idea 3

The first major REST constraint is Client-Server Separation.

---

### Key Idea 4

Clients should not directly interact with databases.

---

### Key Idea 5

Separation allows systems to evolve independently.

---

# Deep Understanding Check

### Why couldn't clients directly access databases?

Security, scalability, and maintenance problems.

---

### Why is client-server separation important?

It isolates responsibilities.

---

### What happens if database schema changes?

Server adapts; clients remain stable.

---

### Hidden tradeoff?

Extra network communication.

---

### What problem appears at scale?

Millions of clients become impossible to coordinate without a server layer.

---

### What would a senior engineer worry about?

API stability and independent evolution of systems.

---

# Quick Questions

1. What is a REST constraint?
2. Why do constraints exist?
3. What problem does client-server separation solve?
4. Why shouldn't clients access databases directly?
5. What advantage does independent evolution provide?

---

# Practical Exercise

Take any app:

* WhatsApp
* Instagram
* Swiggy
* Amazon

Identify:

```text
Client
Server
Database
```

for that application.

---

# Mini Assignment

Draw:

```text
Mobile App
     ↓
API Server
     ↓
Database
```

Then write:

* Why each layer exists
* What responsibility each layer owns

---

# Real-World Scenario

Your company allows mobile apps to directly query databases.

After 3 years:

* Android app versions differ
* Database schema changes
* Millions of users exist

List at least 5 problems that would occur.

---

In **Part 3**, we'll cover another major REST constraint:

> Statelessness

This is one of the most misunderstood concepts in REST and is critical for scalability.

**Are you ready for the next part?**
