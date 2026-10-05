# Prototype Property and Prototypal Inheritance

## 1. What is a prototype?

In JavaScript, a **prototype is simply another object** that an object can delegate property and method lookup to.

Think:

```text
object A
   ↓
object B
   ↓
object C
   ↓
null
```

If JavaScript looks for something inside `A`:

1. Look inside `A`.
2. If found → use it.
3. If not found → look at `A`'s prototype.
4. Continue through the chain.
5. Stop at `null`.

That searching process is the **prototype chain**.

### Very important idea

A prototype is **not a copy** of the object.

It's a relationship:

```text
admin
  ↓
 user
```

Meaning:

> "If I don't have this property, look in `user`."

`Object.create(user)` does **not copy** `user.name` into `admin`; it creates a prototype relationship.

---

## 2. A simple example

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);

admin.role = "admin";
```

We can visualize it as:

```text
        admin
     ┌───────────┐
     │ role      │ → "admin"
     └───────────┘
          │
          │ prototype
          ↓
        user
     ┌───────────┐
     │ name      │ → "John"
     └───────────┘
```

Now:

```javascript
admin.role
```

JavaScript finds `role` directly inside `admin`.

But:

```javascript
admin.name
```

There is no `name` in `admin`.

So JavaScript goes:

```text
admin
  ↓
user
```

and finds `name`.

That's **prototype delegation**.

---

## 3. Prototype delegation

This is the most important concept.

Suppose:

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);
```

When you do:

```javascript
admin.name
```

JavaScript effectively searches:

```text
Does admin have "name"?
        ↓
       NO
        ↓
Does admin's prototype have "name"?
        ↓
       YES
        ↓
      "John"
```

This is called **delegation**.

The `admin` object delegates the lookup to its prototype.

So you can mentally think:

> "I don't have this property. Maybe my prototype has it."

---

## 4. What is the prototype chain?

The chain is simply the sequence of prototype relationships JavaScript follows.

For example:

```text
object
  ↓
prototype
  ↓
another prototype
  ↓
Object.prototype
  ↓
null
```

Suppose:

```javascript
const grandParent = {
  species: "human"
};

const parent = Object.create(grandParent);

const child = Object.create(parent);
```

The relationship is:

```text
child
  ↓
parent
  ↓
grandParent
  ↓
Object.prototype
  ↓
null
```

If you ask:

```javascript
child.species
```

JavaScript searches:

```text
child
 ↓
parent
 ↓
grandParent
 ↓
"human"
```

It stops as soon as it finds the property.

This is the prototype lookup process.

---

## 5. What is `prototype`?

Now we get to one of the confusing parts.

**`prototype` is a property that exists primarily on constructor functions.**

For example:

```javascript
function User(name) {
  this.name = name;
}
```

The function `User` has a property called:

```javascript
User.prototype
```

That object can contain methods that instances of `User` can access.

For example:

```javascript
User.prototype.greet = function () {
  console.log("Hello");
};
```

Then objects created with:

```javascript
const user1 = new User("John");
const user2 = new User("Mike");
```

can access:

```javascript
user1.greet();
user2.greet();
```

even though `greet` isn't directly stored inside `user1` or `user2`.

Conceptually:

```text
user1 ──────┐
            │
user2 ──────┼──→ User.prototype
            │          │
user3 ──────┘          ↓
                     greet()
```

This allows all instances to share a single method definition in memory.

---

## 6. Then what is `__proto__`?

This is where beginners commonly get confused.

`__proto__` and `prototype` are **not the same thing**.

### The `prototype` Property

Usually think:

> "The object that a constructor function provides for its instances."

Example:

```javascript
User.prototype
```

### The `__proto__` Accessor

Think:

> "The prototype of this particular object."

Example:

```javascript
user1.__proto__
```

So:

```text
User function
     │
     │ .prototype
     ↓
User.prototype
     ↑
     │ object's prototype
     │
   user1
```

More specifically:

```javascript
user1.__proto__ === User.prototype
```

is `true` for an object created with:

```javascript
const user1 = new User();
```

---

## 7. The easiest way to remember them

Think about the **direction**.

### Direction: From Constructor (`prototype`)

Starts from the constructor:

```text
User
 ↓
.prototype
 ↓
object used as prototype
```

### Direction: From Object (`__proto__`)

Starts from an object:

```text
user1
 ↓
.__proto__
 ↓
its prototype
```

So:

```javascript
User.prototype
```

and:

```javascript
user1.__proto__
```

can point to the **same object**, but they are accessed from different places.

---

## 8. `prototype` vs `__proto__`

||`prototype`|`__proto__`|
|---|---|---|
|Usually found on|Constructor functions|Objects|
|Purpose|Prototype object used for instances|Points/accesses an object's prototype|
|Example|`User.prototype`|`user1.__proto__`|
|Direction|Constructor → prototype|Object → prototype|

One important modern-JS note: `__proto__` is a legacy accessor. For code, prefer:

```javascript
Object.getPrototypeOf(object)
```

instead of relying on `__proto__`.

For example:

```javascript
Object.getPrototypeOf(user1)
```

gets the same prototype relationship in a standard API.

---

## 9. How `new` connects everything

Constructor functions and `new` establish this connection.

Suppose:

```javascript
function User(name) {
  this.name = name;
}

User.prototype.greet = function () {
  console.log("Hello");
};
```

Then:

```javascript
const user1 = new User("John");
```

Conceptually, `new` establishes this relationship:

```text
             User
              │
              │ .prototype
              ↓
       User.prototype
              ↑
              │ prototype relationship
              │
            user1
```

So:

```javascript
user1.greet();
```

works because JavaScript can't find `greet` directly on `user1`, so it searches its prototype.

ES6 classes also use this same prototype mechanism underneath.

---

## 10. Why don't we put methods directly on every object?

Imagine:

```javascript
const user1 = {
  name: "John",
  greet: function () {
    console.log("Hello");
  }
};

const user2 = {
  name: "Mike",
  greet: function () {
    console.log("Hello");
  }
};
```

Now we have two separate `greet` functions.

With prototypes, we can have:

```text
user1 ──┐
        │
user2 ──┼──→ User.prototype
        │          │
user3 ──┘          ↓
                  greet()
```

All users can delegate to the **same method**.

That's one major reason prototypes are useful.

---

## 11. Property shadowing

Another important prototype concept is **shadowing**.

Suppose:

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);
```

Initially:

```javascript
admin.name
```

gives:

```text
"John"
```

because it comes from the prototype.

But now:

```javascript
admin.name = "Mike";
```

Now:

```javascript
admin.name
```

gives:

```text
"Mike"
```

because JavaScript finds `name` directly on `admin`.

```text
admin
 ├── name: "Mike"  ← found first
 │
 ↓
user
 └── name: "John"
```

The `admin.name` **shadows** `user.name`.

---

## 12. What is prototypal inheritance?

Now we can define it properly.

**Prototypal inheritance is the mechanism where an object gets access to properties and methods through another object in its prototype chain.**

For example:

```text
admin
  ↓
user
```

`admin` can access properties from `user` through delegation.

This is why JavaScript's underlying inheritance model is:

```text
object
  ↓
object
```

rather than primarily:

```text
class
  ↓
class
```

### Important distinction

When people say:

> "admin inherits from user"

don't imagine:

```text
copy user properties → admin
```

Instead think:

```text
admin
  ↓
user
```

and:

> "If admin doesn't have it, look at user."

That's why **delegation** is such a useful mental model.

---

## 13. Prototypal inheritance vs prototype chain

These two terms are related but describe different things.

### Prototypal inheritance

The **mechanism/relationship**:

```text
admin → user
```

Admin can access things from user through the prototype relationship.

### Prototype chain

The **whole path** JavaScript follows:

```text
admin
  ↓
user
  ↓
Object.prototype
  ↓
null
```

So:

> **Prototypal inheritance is the behavior.**  
> **The prototype chain is the path used to perform that behavior.**

---

## 14. Built-in objects also use prototypes

This is extremely important.

You have already been using prototypes without necessarily realizing it.

For example:

```javascript
const arr = [1, 2, 3];
```

`arr` can do:

```javascript
arr.push(4);
arr.map(...);
arr.includes(2);
```

But you didn't define those methods yourself.

Where do they come from?

Conceptually:

```text
arr
 ↓
Array.prototype
 ↓
Object.prototype
 ↓
null
```

So when JavaScript sees:

```javascript
arr.push
```

it searches:

```text
Does arr have push?
       ↓
      NO
       ↓
Does Array.prototype have push?
       ↓
      YES
```

---

## 15. Other built-in objects

The same idea applies to many built-ins.

### Array

```text
myArray
   ↓
Array.prototype
   ↓
Object.prototype
   ↓
null
```

So methods like:

```javascript
push()
map()
filter()
includes()
```

come through the prototype system.

---

### String

```javascript
const name = "John";
```

You can do:

```javascript
name.toUpperCase();
name.includes("J");
```

Conceptually:

```text
string value
    ↓
String.prototype
    ↓
Object.prototype
    ↓
null
```

---

### Date

```javascript
const date = new Date();
```

Conceptually:

```text
date
 ↓
Date.prototype
 ↓
Object.prototype
 ↓
null
```

---

### Function

Functions are objects too.

For example:

```javascript
function greet() {}
```

The function itself has a prototype relationship as an object.

This is one reason JavaScript's prototype system can initially feel confusing: **functions are objects, and functions can also have a `.prototype` property.**

---

## 16. `Object.prototype`

At the top of many ordinary object prototype chains you'll find:

```javascript
Object.prototype
```

For example:

```javascript
const user = {
  name: "John"
};
```

Conceptually:

```text
user
 ↓
Object.prototype
 ↓
null
```

That's why ordinary objects can access methods such as:

```javascript
user.toString()
```

even though you didn't write `toString` inside `user`.

JavaScript searches the prototype chain.

---

## 17. One big picture

Put everything together:

```text
                     Constructor
                         │
                         │ .prototype
                         ↓
                  User.prototype
                  ┌──────────────┐
                  │ greet()      │
                  └──────────────┘
                         ↑
                         │ prototype
                         │
                       user1
                  ┌──────────────┐
                  │ name: "John" │
                  └──────────────┘
                         │
                         ↓
                  Object.prototype
                         │
                         ↓
                        null
```

And the important vocabulary:

```text
prototype
   ↓
an object used as another object's prototype

prototype chain
   ↓
the sequence of prototype objects searched during lookup

prototypal inheritance
   ↓
objects getting access through prototype delegation

.prototype
   ↓
property commonly found on constructor functions

.__proto__
   ↓
legacy accessor for an object's prototype
```

---

## 18. How `class` fits into all of this

When you write:

```javascript
class User {
  greet() {
    console.log("Hello");
  }
}
```

it may look like traditional class-based OOP.

But underneath, JavaScript still uses prototypes.

Conceptually:

```text
user1 ──┐
        │
user2 ──┼──→ User.prototype
        │         │
user3 ──┘         ↓
                greet()
```

So:

```javascript
user1.greet();
```

can find `greet` through the prototype chain.

That's why the statement:

> **"JavaScript classes are built on top of prototypes."**

is fundamental.

---

## 19. The mental model I want you to keep

Don't memorize dozens of definitions.

Think about **lookup**:

```text
             property lookup
                    │
                    ↓
              ┌─────────┐
              │ object  │
              └────┬────┘
                   │
             not found?
                   ↓
              ┌─────────┐
              │prototype│
              └────┬────┘
                   │
             not found?
                   ↓
              ┌─────────┐
              │prototype│
              └────┬────┘
                   │
                   ↓
                 null
```

That's the heart of the entire topic.

When you see:

```javascript
user.greet()
```

ask yourself:

> **"Where does JavaScript find `greet`?"**

It might be:

```text
user
 ↓
User.prototype
 ↓
Object.prototype
```

That question will make prototypes much easier to understand.

### Key Takeaway

- **Prototype = another object that an object can delegate property/method lookup to.**
- **`prototype` and `__proto__` are different:** `.prototype` is commonly the prototype object provided by a constructor; `__proto__` refers to an object's prototype.
- **Prototype chain = the path JavaScript searches.**
- **Prototypal inheritance = the delegation mechanism along that chain.**
- **Built-in objects like Arrays, Strings, Dates, and ordinary Objects also use prototypes.**
- **`class` does not replace prototypes; it provides a cleaner syntax over the prototype system.**
