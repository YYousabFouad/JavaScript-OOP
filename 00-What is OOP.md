# What is OOP?

**OOP = Object-Oriented Programming.**

It is a way of organizing your program around **objects** rather than just
functions and variables.

Think of an object as a **real-world thing** that has:

- **Data** → what it knows
- **Behavior** → what it can do

## Example: A Bank Account

A bank account has data:

```text
balance
owner
accountNumber
```

And it can do things:

```javascript
deposit()
withdraw()
transfer()
```

So instead of keeping everything separately:

```javascript
balance = 2000
owner = "John"

deposit()
withdraw()
```

OOP lets you group related data and behavior together:

```text
BankAccount
 ├── balance
 ├── owner
 ├── accountNumber
 ├── deposit()
 ├── withdraw()
 └── transfer()
```

That's the basic idea.

---

## Why do we need OOP?

Imagine your project eventually has **1,000 users**.

Without a good structure, you might end up with many separate variables:

```javascript
user1Name
user1Balance
user1Movements

user2Name
user2Balance
user2Movements

user3Name
user3Balance
user3Movements
```

That becomes difficult to manage.

With OOP, you can have one **blueprint** for an account:

```text
BankAccount
     ↓
 ┌─────────────┐
 │ name        │
 │ balance     │
 │ movements   │
 │ deposit()   │
 │ withdraw()  │
 └─────────────┘
```

Then create many objects from it:

```text
account1 → BankAccount
account2 → BankAccount
account3 → BankAccount
```

Each object has its **own data**, but they share the same structure and behavior.

---

## Class vs Object

This is one of the most important ideas in OOP.

### Class = Blueprint

A class describes **what an object should look like and what it can do**.

For example:

```text
BankAccount
```

is a blueprint.

### Object = Actual thing

An object is an actual instance created from that blueprint.

```text
account1
account2
account3
```

So:

```text
        Class
     BankAccount
          │
     ┌────┼────┐
     ↓    ↓    ↓
 account1 account2 account3
```

A useful analogy:

> **Class = house blueprint**  
> **Object = actual house**

You can create many houses from the same blueprint.

---

## The 4 main ideas of OOP

You will usually hear about these:

1. **Encapsulation**
2. **Abstraction**
3. **Inheritance**
4. **Polymorphism**

Don't worry about memorizing them yet. They are concepts that you'll understand
gradually when you start writing classes.

For example, **encapsulation** is about keeping an object's data and behavior
together and controlling how that data is accessed.

---

## OOP in JavaScript

JavaScript supports OOP.

For example, JavaScript has:

```javascript
class BankAccount {

}
```

Then you can create objects from it.

```text
class BankAccount
       ↓
   new BankAccount()
       ↓
    object
```

You will later learn things like:

- `class`
- `constructor`
- `this`
- methods
- `new`
- inheritance
- prototypes
- private fields

And **`this` and prototypes are especially important in JavaScript OOP**.

### Key Takeaway: OOP Fundamentals

- **OOP is a programming style that organizes code around objects.**
- **Class = blueprint; Object = actual instance created from that blueprint.**

---

## What is an API?

**API** stands for **Application Programming Interface**.

In simple terms:

> An API is a way for **one program to communicate with another program**.

Think of it like a **waiter in a restaurant**:

```text
You                Waiter/API              Kitchen/Server
 │                     │                       │
 │  "Give me pizza"    │                       │
 │────────────────────>│                       │
 │                     │  Request              │
 │                     │──────────────────────>│
 │                     │                       │
 │                     │      Pizza            │
 │                     │<──────────────────────│
 │      Pizza          │                       │
 │<────────────────────│                       │
```

You don't go into the kitchen yourself.

You tell the **waiter** what you want, and the waiter communicates with the kitchen.

An API works similarly.

---

### In JavaScript

Imagine your website needs a list of jobs from a server.

Your JavaScript sends a **request**:

```text
JavaScript
    ↓
   API
    ↓
Server / Database
```

The server processes the request and sends back a **response**:

```text
Server / Database
    ↓
   API
    ↓
JavaScript
```

The response is often in **JSON**:

```json
[
  {
    "company": "Google",
    "position": "Frontend Developer"
  },
  {
    "company": "Microsoft",
    "position": "JavaScript Developer"
  }
]
```

Your JavaScript can then use that data to update the DOM.

---

## API ≠ Database

This is an important distinction.

```text
Your JavaScript
      ↓
     API
      ↓
   Backend
      ↓
   Database
```

Your JavaScript usually **doesn't directly talk to the database**.

The API provides a controlled way for your application to communicate with the backend.

---

## The two important ideas

When working with APIs, you'll frequently hear:

### Request

What your application **asks for**.

For example:

```http
GET /jobs
```

Meaning:

> "Give me the jobs."

### Response

What the server **sends back**.

For example:

```json
[
  {
    "id": 1,
    "company": "Google"
  }
]
```

---

## How JavaScript communicates with an API

JavaScript commonly uses `fetch()`:

```text
fetch()
   ↓
HTTP Request
   ↓
API
   ↓
HTTP Response
   ↓
JSON data
   ↓
JavaScript
```

You'll learn `fetch()` next, because it's one of the most important tools for
working with APIs in JavaScript.

### Key Takeaway: Web APIs

- **API = a communication interface between programs.**
- **Request → API → Server → Response** is the basic cycle.

so i can say api is the code outside the objectS?

Not quite. You're mixing two different ideas.

An **API is not simply code outside objects**.

Think of an API as a **defined way to interact with something**.

For example, JavaScript itself provides APIs:

```javascript
document.querySelector(".btn");
```

Here, `document` provides an API for interacting with the DOM.

And when you use a web API:

```text
Your JavaScript
      ↓
     API
      ↓
External server
```

The API defines **how you can ask the server for data and what kind of
response you'll get**.

### A useful definition for your notes

> **API = a set of rules and methods that allows one piece of software to
> communicate with and use another piece of software.**

One question for you:

If `fetch()` allows your JavaScript to communicate with a server, **what do
you think `fetch()` is doing in that communication?**

### Key Takeaway: APIs vs Objects

- ❌ API ≠ code outside objects.
- ✅ API = a defined interface for communicating with or using another system.
