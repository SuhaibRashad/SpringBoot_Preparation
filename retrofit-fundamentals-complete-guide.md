
# Retrofit — From Zero to Expert Mental Model

---

# 1. What Problem Is Retrofit Trying To Solve?

Before Retrofit existed, Android/Java developers had to manually perform HTTP requests.

Example:

```java
URL url = new URL("https://api.example.com/users");

HttpURLConnection connection =
    (HttpURLConnection) url.openConnection();

connection.setRequestMethod("GET");

InputStream input =
    connection.getInputStream();

BufferedReader reader =
    new BufferedReader(
        new InputStreamReader(input)
    );
```

Lots of code.

Lots of error handling.

Lots of boilerplate.

---

## Real World Analogy

Suppose you want food delivered.

Without Retrofit:

```text
Go to restaurant
Place order
Wait
Collect food
Return home
```

You do everything manually.

---

With Retrofit:

```text
Open Swiggy/Zomato
Click Order
Done
```

Someone else handles the complexity.

Retrofit does the same thing for HTTP communication.

---

# What Problem It Solves

Removes the complexity of making HTTP requests.

# Why It Exists

Developers were writing repetitive networking code.

# Interview Answer

Retrofit is a type-safe HTTP client for Java and Android that simplifies REST API communication.

# One-Line Notes

```text
Retrofit automates HTTP communication.
```

---

# 2. What Is HTTP?

Before Retrofit, we must understand HTTP.

---

HTTP = HyperText Transfer Protocol

Protocol = agreed communication rules.

---

Client:

```text
Mobile App
```

Server:

```text
Backend API
```

Communication:

```text
Client
   |
   | HTTP Request
   |
   v
Server

Server
   |
   | HTTP Response
   |
   v
Client
```

---

Example:

Request:

```http
GET /users/1
```

Meaning:

```text
Give me user 1
```

Response:

```json
{
  "id":1,
  "name":"Imran"
}
```

---

# What Problem It Solves

Provides a standard communication method.

# Why It Exists

Different systems need common communication rules.

# Interview Answer

HTTP is a request-response protocol used for communication between clients and servers.

# One-Line Notes

```text
HTTP defines how clients and servers communicate.
```

---

# 3. What Is REST API?

New term.

Must define it first.

---

REST = Representational State Transfer

In practice:

REST API = URL endpoints that provide data.

Example:

```text
GET /users
GET /users/1
POST /users
DELETE /users/1
```

Think of it as a menu in a restaurant.

---

Restaurant Menu:

```text
Pizza
Burger
Coffee
```

API Menu:

```text
/users
/orders
/products
```

---

# What Problem It Solves

Provides organized access to backend functionality.

# Why It Exists

Clients need predictable URLs to access resources.

# Interview Answer

A REST API exposes resources through HTTP endpoints.

# One-Line Notes

```text
REST API = HTTP endpoints exposing resources.
```

---

# 4. Where Retrofit Fits

Without Retrofit:

```text
Application
    ↓
HttpURLConnection
    ↓
HTTP
    ↓
Server
```

---

With Retrofit:

```text
Application
    ↓
Retrofit
    ↓
HTTP
    ↓
Server
```

Retrofit becomes a layer between your code and HTTP.

---

# What Problem It Solves

Hides low-level networking details.

# Why It Exists

Developers should focus on business logic instead of networking internals.

# Interview Answer

Retrofit abstracts low-level HTTP communication behind simple Java interfaces.

# One-Line Notes

```text
Retrofit sits between application code and HTTP.
```

---

# 5. Retrofit Architecture

Internally:

```text
Application
      ↓
Retrofit
      ↓
OkHttp
      ↓
HTTP
      ↓
Server
```

---

## New Concept: OkHttp

Retrofit does NOT send network requests itself.

It uses another library:

```text
OkHttp
```

Think:

```text
Retrofit = Manager

OkHttp = Worker
```

---

Retrofit says:

```text
Send GET request
```

OkHttp actually sends it.

---

# What Problem It Solves

Separates API definition from network implementation.

# Why It Exists

Retrofit specializes in API mapping while OkHttp specializes in networking.

# Interview Answer

Retrofit internally uses OkHttp as its networking engine.

# One-Line Notes

```text
Retrofit delegates actual network work to OkHttp.
```

---

# 6. Retrofit Dependency

Gradle:

```gradle
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
```

---

Meaning:

```text
Download Retrofit library
Add it to project
```

---

Without this:

```java
Retrofit retrofit;
```

Compilation error:

```text
Cannot resolve symbol Retrofit
```

---

# What Problem It Solves

Makes Retrofit available to the project.

# Why It Exists

Java cannot use libraries that are not added as dependencies.

# Interview Answer

The Retrofit dependency imports Retrofit classes into the project.

# One-Line Notes

```text
Dependency = make Retrofit available.
```

---

# 7. Creating Retrofit Instance

```java
Retrofit retrofit =
    new Retrofit.Builder()
        .baseUrl("https://api.example.com/")
        .build();
```

---

## Line By Line

### Builder

```java
new Retrofit.Builder()
```

Creates configuration object.

Think:

```text
Configure before constructing.
```

---

If removed:

```java
new Retrofit()
```

Impossible.

Retrofit uses Builder Pattern.

---

### baseUrl

```java
.baseUrl("https://api.example.com/")
```

Base server URL.

---

Without it:

Runtime error.

Retrofit doesn't know where requests go.

---

### build

```java
.build()
```

Creates Retrofit instance.

---

Without it:

Only configuration exists.

No Retrofit object.

---

# Internal Flow

```text
Builder Created
      ↓
Configuration Added
      ↓
Build Called
      ↓
Retrofit Object Created
```

---

# 8. API Interface

Most important Retrofit concept.

---

Create interface:

```java
public interface UserApi {

    @GET("users/1")
    Call<User> getUser();

}
```

---

Question:

Where is implementation?

You never wrote one.

---

Answer:

Retrofit generates implementation at runtime.

---

Think:

```text
Interface = Contract

Retrofit = Employee following contract
```

---

# Internal Flow

```text
Your Interface
       ↓
Retrofit Reads Annotations
       ↓
Generates Implementation
       ↓
Creates HTTP Request
```

---

# What Problem It Solves

Removes manual request construction.

# Why It Exists

Developers describe APIs declaratively.

# Interview Answer

Retrofit dynamically generates API implementations from annotated interfaces.

# One-Line Notes

```text
Retrofit creates interface implementation automatically.
```

---

# 9. What Are Annotations?

New concept.

---

Annotation:

```java
@GET
@POST
@Path
@Query
```

Metadata attached to code.

---

Example:

```java
@GET("users")
Call<List<User>> getUsers();
```

Meaning:

```text
When this method is called,
perform HTTP GET on /users
```

---

Without annotation:

Retrofit doesn't know:

* URL
* HTTP method
* Parameters

---

# Common Annotations

## GET

```java
@GET("users")
```

Read data.

---

## POST

```java
@POST("users")
```

Create data.

---

## DELETE

```java
@DELETE("users/{id}")
```

Delete data.

---

## PUT

```java
@PUT("users/{id}")
```

Update data.

---

# 10. Path Parameters

URL:

```text
/users/10
```

Dynamic value:

```java
@GET("users/{id}")
Call<User> getUser(
    @Path("id") int id
);
```

Call:

```java
api.getUser(10);
```

Generated URL:

```text
/users/10
```

---

Internal Flow:

```text
{id}
 ↓
10
 ↓
users/10
```

---

# 11. Query Parameters

URL:

```text
/users?page=1
```

Retrofit:

```java
@GET("users")
Call<List<User>> getUsers(
    @Query("page") int page
);
```

Call:

```java
getUsers(1);
```

Generated:

```text
/users?page=1
```

---

# Path vs Query

| Path              | Query               |
| ----------------- | ------------------- |
| /users/10         | /users?page=1       |
| Specific resource | Filtering/searching |

---

# 12. Gson Converter

Server returns:

```json
{
  "id":1,
  "name":"Imran"
}
```

Retrofit cannot create Java objects.

Need Gson.

---

Dependency:

```gradle
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```

---

Configuration:

```java
.addConverterFactory(
    GsonConverterFactory.create()
)
```

---

Without this:

Retrofit receives JSON.

But cannot convert it.

---

Flow:

```text
JSON
   ↓
Gson
   ↓
User Object
```

---

# 13. Complete Request Flow

Suppose:

```java
api.getUser(1);
```

---

Internally:

```text
Step 1
Method called

getUser(1)

      ↓

Step 2
Retrofit reads annotation

@GET("users/{id}")

      ↓

Step 3
Replace {id}

users/1

      ↓

Step 4
Create HTTP Request

GET /users/1

      ↓

Step 5
OkHttp sends request

      ↓

Step 6
Server processes

      ↓

Step 7
Server returns JSON

      ↓

Step 8
Retrofit receives response

      ↓

Step 9
Gson converts JSON

      ↓

Step 10
User object returned
```

---

# Common Beginner Mistakes

## Missing Trailing Slash

Wrong:

```java
.baseUrl("https://api.com")
```

Error:

```text
Base URL must end in /
```

Correct:

```java
.baseUrl("https://api.com/")
```

---

## Missing Converter

```java
.build();
```

without:

```java
.addConverterFactory(...)
```

Result:

```text
Cannot deserialize JSON automatically
```

---

## Wrong POJO

JSON:

```json
{
  "userName":"Imran"
}
```

POJO:

```java
String name;
```

Result:

```java
name = null
```

Field names don't match.

---

# Retrofit vs HttpURLConnection

| Feature         | HttpURLConnection | Retrofit  |
| --------------- | ----------------- | --------- |
| Manual HTTP     | Yes               | No        |
| JSON Conversion | Manual            | Automatic |
| API Interface   | No                | Yes       |
| Boilerplate     | High              | Low       |
| Readability     | Poor              | Excellent |

---

# Complete Mental Model

```text
Application
      ↓
API Interface
      ↓
Retrofit
      ↓
Annotations
      ↓
HTTP Request Creation
      ↓
OkHttp
      ↓
Network
      ↓
Server
      ↓
JSON Response
      ↓
OkHttp
      ↓
Retrofit
      ↓
Gson Converter
      ↓
POJO
      ↓
Application
```

# Final Interview Answer

> Retrofit is a type-safe HTTP client for Java and Android. It allows developers to define REST APIs using interfaces and annotations. Retrofit internally uses OkHttp for network communication and can use converters such as Gson or Jackson to automatically serialize Java objects into JSON and deserialize JSON responses into POJOs.
