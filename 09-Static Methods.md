# Static Methods

A **static method** is a method that belongs to the **class itself**, not to the objects (instances) created from that class.

Think of it like this:

```text
Class
 │
 ├── static method()  ← belongs to the class
 │
 └── instance methods ← belong to objects
        │
        ├── object 1
        └── object 2
```

---

## 1. Normal method

A normal method is available on instances:

```javascript
class User {
  sayHello() {
    console.log("Hello");
  }
}

const user = new User();

user.sayHello(); // works
```

Here:

```javascript
user.sayHello()
```

works because `sayHello()` is an **instance method**.

But:

```javascript
User.sayHello(); // ❌
```

doesn't work.

---

## 2. Static method

You create a static method using the `static` keyword:

```javascript
class User {
  static sayHello() {
    console.log("Hello");
  }
}
```

Now you call it through the **class**:

```javascript
User.sayHello(); // works
```

But you **cannot** call it through an instance:

```javascript
const user = new User();

user.sayHello(); // ❌
```

So the key difference is:

|Method|Called through|
|---|---|
|Normal method|`object.method()`|
|Static method|`Class.method()`|

---

## 3. Why would we use static methods?

Static methods are useful when the operation is related to the **class as a whole**, rather than one specific object.

For example:

```javascript
class MathHelper {
  static add(a, b) {
    return a + b;
  }
}

MathHelper.add(5, 3);
```

You don't need to create a `MathHelper` object just to perform an addition.

```javascript
const helper = new MathHelper(); // unnecessary
```

The operation doesn't depend on a particular `MathHelper` object, so making it static makes sense.

---

## 4. Important: `this` inside static methods

Inside a static method, `this` refers to the **class itself**.

```javascript
class User {
  static type = "User";

  static getType() {
    console.log(this.type);
  }
}

User.getType(); // "User"
```

Here:

```javascript
this
```

refers to:

```javascript
User
```

not an instance of `User`.

---

## 5. Think about this: Factory methods

Look at this:

```javascript
class User {
  static createGuest() {
    return new User();
  }

  sayHello() {
    console.log("Hello");
  }
}
```

Why do you think `createGuest()` is static instead of being a normal instance method?

Because you want to create a `User` *before* having an instance yet! It acts as a **factory method** on the class itself.

---

## Key Takeaway

- `static` methods belong to the **class**, not its instances.
- Call them with `Class.method()`, not `object.method()`.
