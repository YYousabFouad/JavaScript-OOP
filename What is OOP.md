**OOP = Object-Oriented Programming.**

It is a way of organizing your program around **objects** rather than just functions and variables.

Think of an object as a **real-world thing** that has:

- **Data** → what it knows
- **Behavior** → what it can do

### Example: A Bank Account

A bank account has data:

```
balance
owner
accountNumber
```

And it can do things:

```
deposit()
withdraw()
transfer()
```

So instead of keeping everything separately:

```
balance = 2000
owner = "John"

deposit()
withdraw()
```

OOP lets you group related data and behavior together:

```
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

Imagine your JobTrack project eventually has **1,000 users**.

Without a good structure, you might end up with many separate variables:

```
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

```
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

```
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

```
BankAccount
```

is a blueprint.

### Object = Actual thing

An object is an actual instance created from that blueprint.

```
account1
account2
account3
```

So:

```
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

Don't worry about memorizing them yet. They are concepts that you'll understand gradually when you start writing classes.

For example, **encapsulation** is about keeping an object's data and behavior together and controlling how that data is accessed.

---

## OOP in JavaScript

JavaScript supports OOP.

For example, JavaScript has:

```
class BankAccount {
    
}
```

Then you can create objects from it.

```
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

### Key Takeaway

- **OOP is a programming style that organizes code around objects.**
- **Class = blueprint; Object = actual instance created from that blueprint.**