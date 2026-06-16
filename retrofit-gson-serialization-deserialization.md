# Gson + Retrofit + Serialization/Deserialization

---

# 1. What Problem Are Gson and Retrofit Trying To Solve?

Imagine you have:

```java
User user = new User(
    1,
    "Imran",
    "imran@gmail.com"
);
```

Now you want to send this object to a server.

Problem:

A Java object only exists inside the JVM.

The internet does NOT understand Java objects.

The network only understands raw data:

```text
Bytes
```

or text:

```text
String
```

So we need a way to convert:

```text
Java Object
      ↓
Something transferable
      ↓
Network
```

This is the problem.

---

## Why Does This Problem Exist?

Because:

```java
User user = new User();
```

contains:

* memory addresses
* JVM-specific information
* object references

A server written in:

* Python
* NodeJS
* Go
* C#

cannot understand a Java object.

So we need a common language.

Most APIs use:

```text
JSON
```

Example:

```json
{
  "id": 1,
  "name": "Imran",
  "email": "imran@gmail.com"
}
```

Every language understands JSON.

---

## Real World Analogy

Suppose:

You speak Tamil.

Server speaks Hindi.

You cannot directly communicate.

Need a translator.

```text
Tamil
 ↓
Translator
 ↓
English
 ↓
Translator
 ↓
Hindi
```

Similarly:

```text
Java Object
 ↓
Gson
 ↓
JSON
 ↓
Network
 ↓
Gson/Jackson
 ↓
Object
```

---

# What Problem It Solves

Convert language-specific objects into a common transferable format.

# Why It Exists

Different systems cannot understand each other's internal object structures.

# Interview Answer

Serialization converts an object into a transferable format such as JSON. Deserialization reconstructs the object from that format.

# One-Line Notes

```text
Objects cannot travel across networks. JSON can.
```

---

# 2. Why Do We Get String Back From HTTP Calls?

Let's understand HTTP first.

---

## What Is HTTP?

HTTP = HyperText Transfer Protocol

Protocol = agreed set of rules.

Example:

You order food.

Restaurant and customer follow a process.

Similarly:

Client and server follow HTTP rules.

---

## Request

```http
GET /users/1
```

Client asks:

```text
Give me user 1
```

---

## Response

Server sends:

```http
200 OK

{
   "id":1,
   "name":"Imran"
}
```

Notice:

Server cannot send Java object.

It sends text.

Internally:

```text
JSON String
       ↓
Bytes
       ↓
Network
```

---

## Why Not Send Object Directly?

Because server doesn't know:

```java
class User {}
```

exists in your JVM.

It only knows:

```text
bytes
```

Therefore:

```text
Object
 ↓
JSON String
 ↓
Bytes
 ↓
Network
```

---

# Internal Flow

```text
Java Object
      ↓
JSON String
      ↓
UTF-8 Bytes
      ↓
HTTP Response
      ↓
Network
      ↓
HTTP Response
      ↓
Bytes
      ↓
JSON String
      ↓
Java Object
```

---

# What Problem It Solves

Allows communication between different systems.

# Why It Exists

Network transfers bytes, not objects.

# Interview Answer

HTTP transfers bytes. JSON is a common text format placed inside those bytes.

# One-Line Notes

```text
HTTP sends bytes, not Java objects.
```

---

# 3. Serialization

Let's define it carefully.

---

## Definition

Serialization = converting an object into a transferable format.

Example:

```java
User user =
    new User(1,"Imran");
```

Convert into:

```json
{
  "id":1,
  "name":"Imran"
}
```

This conversion is serialization.

---

## Internally

Object:

```java
User
 ├─ id = 1
 └─ name = Imran
```

Gson inspects fields:

```java
id
name
```

Creates:

```json
{
  "id":1,
  "name":"Imran"
}
```

---

## Gson Example

```java
Gson gson = new Gson();

String json =
    gson.toJson(user);
```

### Line-by-Line

```java
Gson gson = new Gson();
```

Create Gson converter.

---

```java
gson.toJson(user);
```

Convert object into JSON.

---

Result:

```json
{"id":1,"name":"Imran"}
```

---

## If Serialization Didn't Exist

You would manually create:

```java
String json =
"{\"id\":1,\"name\":\"Imran\"}";
```

Very error-prone.

---

# What Problem It Solves

Converts objects into transferable data.

# Why It Exists

Objects cannot directly cross process boundaries.

# Interview Answer

Serialization transforms an in-memory object into JSON, XML, binary, or another transport format.

# One-Line Notes

```text
Object → JSON
```

---

# 4. Deserialization

Reverse process.

---

Server sends:

```json
{
  "id":1,
  "name":"Imran"
}
```

Need:

```java
User user
```

This reconstruction is deserialization.

---

## Gson Example

```java
User user =
    gson.fromJson(
        json,
        User.class
    );
```

---

### Internally

JSON:

```json
{
  "id":1,
  "name":"Imran"
}
```

Gson sees:

```text
id → 1
name → Imran
```

Creates:

```java
new User();
```

Then:

```java
user.id = 1;
user.name = "Imran";
```

Done.

---

# What Problem It Solves

Turns received data into usable Java objects.

# Why It Exists

Working with objects is easier than parsing strings manually.

# Interview Answer

Deserialization converts JSON into a Java object.

# One-Line Notes

```text
JSON → Object
```

---

# 5. Your Binary Tree Example

Excellent example.

Suppose:

```java
      10
     /  \
    5   20
```

This exists in memory.

Memory structure:

```text
Node
 ├─ value=10
 ├─ left -> address A
 └─ right -> address B
```

Addresses:

```text
0x123
0x456
```

are meaningless on another machine.

You cannot send memory addresses.

---

Need serialization.

Convert tree to:

```json
{
  "value":10,
  "left":{
      "value":5
  },
  "right":{
      "value":20
  }
}
```

Send this JSON.

Server receives it.

Deserializes.

Reconstructs:

```text
      10
     /  \
    5   20
```

This is exactly why serialization exists.

---

# 6. Where Retrofit Comes In

Now another problem appears.

Without Retrofit:

```java
Open connection
Create request
Send request
Receive response
Read bytes
Convert bytes to string
Convert string to object
Handle errors
```

Too much code.

Retrofit automates this.

---

## Retrofit's Job

Retrofit is an HTTP client library.

It handles:

```text
Making HTTP calls
Receiving responses
Calling converters
```

It does NOT itself convert JSON.

It delegates conversion.

---

# Retrofit + Gson Relationship

```text
Retrofit
    |
    | HTTP
    |
Gson Converter
```

Think:

```text
Retrofit = Delivery guy

Gson = Translator
```

---

# Flow

```text
Java Object
      ↓
Gson
      ↓
JSON
      ↓
Retrofit
      ↓
HTTP Request
      ↓
Server
      ↓
HTTP Response
      ↓
Retrofit
      ↓
JSON
      ↓
Gson
      ↓
Java Object
```

---

# 7. POJO

## What Is POJO?

POJO = Plain Old Java Object

Just a normal Java class.

Example:

```java
public class User {

    private int id;
    private String name;

}
```

Nothing special.

No framework code.

No inheritance required.

---

## Why POJO?

Instead of working with:

```json
{
  "id":1,
  "name":"Imran"
}
```

as a string:

```java
String response;
```

you work with:

```java
User user;
```

Much cleaner.

---

# 8. Retrofit Example

## POJO

```java
public class User {

    int id;
    String name;

}
```

---

## API Interface

```java
public interface UserApi {

    @GET("/users/1")
    Call<User> getUser();

}
```

---

## Retrofit Configuration

```java
Retrofit retrofit =
    new Retrofit.Builder()
        .baseUrl("https://api.com/")
        .addConverterFactory(
            GsonConverterFactory.create()
        )
        .build();
```

---

### Line-by-Line

```java
new Retrofit.Builder()
```

Create Retrofit configuration.

---

```java
.baseUrl(...)
```

Base server URL.

---

```java
.addConverterFactory(...)
```

Tell Retrofit:

```text
Use Gson for conversion
```

---

If removed:

```java
.addConverterFactory(...)
```

Retrofit receives:

```json
{
 "id":1
}
```

but doesn't know how to convert it.

You'll get raw response handling instead.

---

```java
.build()
```

Creates Retrofit instance.

---

# Internal Execution Flow

```text
getUser()
    ↓
Retrofit creates HTTP request
    ↓
Request sent
    ↓
Server returns JSON
    ↓
Retrofit receives bytes
    ↓
Bytes → String
    ↓
Gson Converter
    ↓
JSON parsed
    ↓
User object created
    ↓
Returned to application
```

---

# Common Beginner Mistakes

## Mistake 1

JSON field mismatch.

JSON:

```json
{
  "user_name":"Imran"
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

Why?

Field names differ.

---

## Mistake 2

Forget converter.

```java
Retrofit.Builder()
    .baseUrl(...)
    .build();
```

No Gson converter.

Result:

```text
Cannot automatically parse JSON
```

---

## Mistake 3

Wrong POJO structure.

JSON:

```json
{
  "user":{
      "id":1
  }
}
```

POJO:

```java
class User {
   int id;
}
```

Will fail because JSON structure doesn't match object structure.

---

# Retrofit vs Gson

| Feature              | Retrofit | Gson |
| -------------------- | -------- | ---- |
| Makes HTTP calls     | Yes      | No   |
| Sends requests       | Yes      | No   |
| Receives responses   | Yes      | No   |
| Converts JSON        | No       | Yes  |
| Creates Java objects | No       | Yes  |

---

# Complete Mental Model

```text
Application
    |
    | Java Object
    |
    v
Gson
    |
    | JSON String
    |
    v
Retrofit
    |
    | HTTP Request
    |
    v
Network
    |
    | HTTP Response
    |
    v
Retrofit
    |
    | JSON String
    |
    v
Gson
    |
    | Java Object
    |
    v
Application
```

# Copy-Paste Notes

```text
HTTP transfers bytes, not Java objects.

Serialization:
Object → JSON

Deserialization:
JSON → Object

Gson:
Responsible for converting between Java objects and JSON.

Retrofit:
Responsible for making HTTP requests and receiving responses.

POJO:
Plain Java class used to represent JSON data.

Flow:

Object
 ↓
Gson Serialization
 ↓
JSON
 ↓
HTTP Request
 ↓
Network
 ↓
HTTP Response
 ↓
JSON
 ↓
Gson Deserialization
 ↓
POJO

Retrofit = HTTP client
Gson = JSON converter

Without Converter:
Retrofit receives JSON but cannot automatically create Java objects.

Why JSON?
Because every programming language can understand it, while Java objects are JVM-specific.
```

One important correction to your understanding: we do **not always serialize to a String first**. The actual network transmits **bytes**. JSON is a textual representation, and that text is encoded into bytes (usually UTF-8) before being sent. So the more accurate flow is:

```text
Java Object
   ↓
JSON
   ↓
UTF-8 Bytes
   ↓
Network
```

and on the receiving side:

```text
Network Bytes
   ↓
JSON
   ↓
Java Object
```

Gson/Jackson are responsible for the JSON ↔ Object conversion, while Retrofit handles the HTTP communication and invokes those converters automatically.
