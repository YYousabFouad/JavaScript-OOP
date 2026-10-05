# Encapsulation

**Encapsulation** means:

> **Keeping an object's data and the methods that work with that data together,
> while controlling how that data can be accessed or changed.**

Think of an object as a **box**:

```text
┌─────────────────────────┐
│        Object           │
│                         │
│  Data                   │
│  ├── name               │
│  └── balance            │
│                         │
│  Methods                │
│  ├── deposit()          │
│  └── withdraw()         │
└─────────────────────────┘
```

The important idea is that the object can decide **what outside code is
allowed to do with its data**.

## Without Encapsulation

Imagine:

```javascript
const account = {
  balance: 1000,
};

account.balance = -5000;
```

Nothing prevents outside code from directly changing `balance`.

## With Encapsulation

A class can make a property **private** using `#`:

```javascript
class Account {
  #balance = 1000;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

Now:

```javascript
const account = new Account();

account.deposit(500);

console.log(account.getBalance());
```

Outside code **cannot directly access**:

```javascript
account.#balance; // ❌
```

Instead, it interacts with the object through its methods.

## The Key Idea

Encapsulation is **not simply "making properties private."**

It is about creating a controlled boundary:

```text
Outside code
     │
     │ public methods and properties
     ▼
┌───────────────┐
│     Object    │
│               │
│  #private     │ ← internal state
│               │
│  methods()    │ ← controlled access
└───────────────┘
```

The public methods form the object's **interface**: the set of actions other
code is allowed to request. The object keeps the details of how those actions
work inside the class.

## What “Private” Means in JavaScript

A private class member is an implementation detail that can only be used by
the class that declares it. JavaScript enforces this boundary: outside code
cannot read or change a private field, and a different class cannot use it
just because it has a similar name.

Private does not mean encrypted or secret from every tool or person. It means
ordinary JavaScript code must use the class's public interface instead of
accessing that member directly.

## Private Fields: Syntax and Access

Declare a private field inside the class body by putting `#` before its name,
such as `#balance`. Use `this.#balance` from an instance method to refer to
that field on the current object. A private field may be given an initial
value when declared, or declared first and assigned later by the class.

The `#` is part of the member's name. It is different from a regular property
whose name is the string `"#balance"`. Bracket notation such as
`account["#balance"]` does not access the private field.

Inside the declaring class, its methods can read or update the private field.
Outside the class, writing `account.#balance` is invalid JavaScript syntax;
it does not simply evaluate to `undefined`. This is why the class must provide
public operations such as `deposit()` or `getBalance()` when outside code needs
some controlled way to work with the data.

Private fields must be declared in the class body before they are used. They
are not ordinary properties that can be added later with a string key. Each
instance gets its own instance field value.

## Private Methods

A private method uses the same `#` marker: `#methodName()`. The declaring class
can call it from another method with `this.#methodName()`. Code outside that
class cannot call it directly.

Private methods are useful for small internal steps that support a public
operation. For example, a public method might check an input and then call a
private helper to perform one part of the work. Keeping the helper private
means callers depend on the public operation, not on the class's internal
steps. The class can reorganize those steps later without changing its public
interface.

## Public Methods as a Controlled Interface

A public method is a doorway into an object's behavior. The class author
chooses which operations to expose and what each operation returns. A public
method can expose a safe result without exposing the private field itself;
`getBalance()` in the earlier example returns the balance while keeping the
field inaccessible directly.

Public does not mean that a method must allow every requested change. The
method can decide whether an action is valid, reject it, or leave the object
unchanged. This lets the class stay in control of its own data.

## Validation and Invariants

An **invariant** is a rule that should remain true for an object throughout
its lifetime. For an account, an invariant might be that its balance cannot
become negative. For a name, it might be that the value cannot be empty.

If the data is public and freely changeable, outside code can break these
rules. With a private field, the class can check a requested change in its
public method before changing the field. In everyday terms, the method can
check that an amount is a valid positive number before depositing it, or that
a withdrawal will not make the balance negative. The exact rules depend on
what the object represents.

Validation matters at every public entry point that can change the state.
Keeping related changes inside the class makes it easier to reason about
whether the invariant still holds.

## Private Static Members

An ordinary private field belongs to each instance. A **static** member
belongs to the class itself and is shared at the class level. JavaScript also
allows private static fields and methods. Their declaration combines the
`static` keyword with the private name, for example `static #count` or
`static #helper()`.

Use a private static field for class-wide internal state, such as a counter
shared by all instances. Use a private static method for a helper that
supports class-level behavior. These members are accessed through the class
inside its own static methods, rather than through an individual object's
`this`.

Static and instance members are different kinds of members. An instance
method cannot treat a private static field as if it belonged to each object,
and an instance private field is not available as a class-level field.

## Inheritance and Access Limits

Private members belong to the class that declares them. A subclass cannot
directly read or call its parent's private fields or methods. Inheritance does
not turn a parent's private member into a protected member.

If a subclass needs an operation involving the parent's private state, the
parent can expose a suitable public or protected-like method for that
operation. JavaScript has no `protected` keyword for class members, so such a
method is still public and should be designed as part of the class's
interface. A subclass may declare a private member with the same spelling,
but it is a separate private member, not access to the parent's one.

## Common Mistakes

- Trying to use `object.#field` outside the class. The private name is only
  valid within the class that declared it.
- Thinking `object["#field"]` accesses the private field. It looks up a normal
  string-named property instead.
- Using a private field or method without declaring it in that class. Private
  names are declared in the class body; they are not created dynamically.
- Expecting a subclass to access a parent's private member. The declaring
  class must provide an interface for behavior that subclasses need.
- Making a field private but then exposing unrestricted public methods that
  allow invalid changes. Encapsulation works best when public operations
  preserve the object's rules.
- Treating `#private` as encryption. It controls access in JavaScript code; it
  is not a way to store passwords or other secrets securely.

## A Simple Boundary Diagram

```text
Other code
    │
    │ calls a public operation
    ▼
┌──────────────────────────────┐
│ Class                         │
│                               │
│ Public interface: deposit()   │
│          │                    │
│          ▼                    │
│ Private method: #checkAmount()│
│          │                    │
│          ▼                    │
│ Private field: #balance      │
└──────────────────────────────┘
    ▲
    └── result returned to caller
```

The caller asks for an action through the public interface. The class checks
the request and decides how its private state should change.

So when you see:

```javascript
class User {
  #password;

  login(password) {
    // ...
  }
}
```

The idea is:

> "The object owns and controls its internal data. Other code should interact
> with it through the interface I provide."

## Key Takeaway

- **Encapsulation = protecting an object's internal state and controlling how
  outside code interacts with it.**
- JavaScript private fields (`#field`) and methods (`#method()`) keep internal
  details inside their declaring class; public methods provide the controlled
  interface and can preserve the object's rules.
