# Prototypal Inheritance (Prototype Delegation)

The easiest way to understand it is:

> **An object can delegate a property or method lookup to another object.**

That other object is called its **prototype**.

## 1. The Basic Idea

Imagine:

```text
object A
   ↓
prototype object B
   ↓
prototype object C
   ↓
null
```

When JavaScript tries to find a property on `A`:

1. Look inside `A`.
2. If found → use it.
3. If not found → look at `A`'s prototype (`B`).
4. If not found → look at `B`'s prototype (`C`).
5. Continue until `null`.

This is called the **prototype chain**.

---

## 2. A Simple Example

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);

admin.role = "admin";
```

Here:

```text
admin
 ├── role: "admin"
 │
 ↓ prototype
user
 └── name: "John"
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

There is no `name` inside `admin`.

So JavaScript **delegates the lookup** to its prototype:

```text
admin → user
```

It finds `name` in `user`.

So:

```javascript
console.log(admin.name);
```

gets:

```text
"John"
```

---

## 3. Important: The Property Wasn't Copied

This is one of the most important things to understand.

When we do:

```javascript
const admin = Object.create(user);
```

JavaScript does **not** copy `user.name` into `admin`.

Instead:

```text
admin
  ↓
user
```

`admin` has a relationship with `user`.

You can think of it as:

> "If I don't have what you're looking for, ask my prototype."

That's why **delegation** is another useful name for this behavior.

---

## 4. What Happens When You Change the Prototype?

Suppose:

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);
```

Then:

```javascript
user.name = "Mike";
```

Now:

```javascript
admin.name
```

will give:

```text
"Mike"
```

Why?

Because `admin` is still looking at `user`.

It isn't holding a copied version of `name`.

---

## 5. What If the Child Has the Same Property?

This is called **property shadowing**.

```javascript
const user = {
  name: "John"
};

const admin = Object.create(user);

admin.name = "Mike";
```

Now:

```javascript
admin.name
```

returns:

```text
"Mike"
```

JavaScript finds `name` immediately on `admin`, so it doesn't need to look at
the prototype.

```text
admin
 ├── name: "Mike"   ← found here
 │
 ↓
user
 └── name: "John"
```

The `user.name` is **shadowed** by `admin.name`.

---

## 6. Why Is This Useful for Methods?

Imagine several objects need the same method.

Instead of putting a separate copy of the method on every object, they can
delegate to a shared prototype.

Conceptually:

```text
user1 ──┐
        │
user2 ──┼──→ User prototype
        │        └── greet()
user3 ──┘
```

When:

```javascript
user1.greet()
```

JavaScript looks for `greet`.

If `user1` doesn't have it:

```text
user1
  ↓
User prototype
  ↓
greet()
```

So all the objects can use the same method.

This is one of the major reasons prototypes are important in JavaScript.

---

## 7. How This Connects to `class`

When you write:

```javascript
class User {
  greet() {
    console.log("Hello");
  }
}
```

You might think every object gets its own `greet` function.

But JavaScript actually uses prototypes behind the scenes.

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

This is why **JavaScript classes are built on top of the prototype system**.

---

## 8. Prototypal Inheritance vs Classical Inheritance

This distinction is useful.

In languages such as Java/C++ you commonly think:

```text
Dog
 ↓
Animal
```

where a class inherits from another class.

JavaScript's underlying mechanism is different:

```text
dog object
   ↓
animal object
```

An object can delegate property/method lookup to another object.

That's **prototypal inheritance**.

So a good mental model is:

> **JavaScript inheritance is fundamentally object-to-object delegation through
> the prototype chain.**

`class` gives us a more familiar syntax for working with that system.

---

## Key Takeaway

- **Prototype delegation:** if an object doesn't have a property/method,
  JavaScript looks for it in its prototype.
- **Prototype chain:** the path JavaScript follows while searching for that
  property/method.
- `class` in JavaScript is built on top of this prototype mechanism.
