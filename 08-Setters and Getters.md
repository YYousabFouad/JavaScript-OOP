# Setters and Getters

In JavaScript, **getters and setters** are special methods that let you control how a property is **read** and **changed**.

They are commonly used with **classes** and are closely related to **encapsulation**.

---

## 1. Getter — `get`

A getter runs when you **read** a property.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  get username() {
    return this.name;
  }
}

const user = new User("Ahmed");

console.log(user.username);
```

Notice something important:

```javascript
user.username
```

not:

```javascript
user.username()
```

Even though `username` is implemented as a method, JavaScript lets you use it like a property.

So:

```javascript
get username() {
  return this.name;
}
```

means:

> "When someone tries to read `username`, execute this code and return the result."

---

## 2. Setter — `set`

A setter runs when you **assign a value** to a property.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  set username(value) {
    this.name = value;
  }
}

const user = new User("Ahmed");

user.username = "Mohamed";
```

This:

```javascript
user.username = "Mohamed";
```

automatically calls:

```javascript
set username(value)
```

with:

```javascript
value = "Mohamed"
```

So a setter allows you to control what happens when someone changes a property.

---

## 3. Why are they useful?

The real power is that you can put **validation and logic** inside them.

For example, imagine you don't want a user's age to be negative:

```javascript
class User {
  constructor(age) {
    this.age = age;
  }

  set userAge(value) {
    // validation logic here
  }
}
```

Now the outside code doesn't directly mutate internal state without checks.

That's useful for **encapsulation**:

```text
Outside code
     │
     ▼
 user.userAge = 25
     │
     ▼
   setter
     │
     ▼
 validation / logic
     │
     ▼
 internal data
```

---

## 4. Getter + Setter together

Usually they work as a pair:

```javascript
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    this._name = value;
  }
}
```

Then:

```javascript
const user = new User("Ahmed");

console.log(user.name); // getter

user.name = "Mohamed";  // setter

console.log(user.name); // getter
```

The `_name` is commonly used as the **internal storage property**, while `name` is the public interface.

### Important distinction

A normal method:

```javascript
getName() {
  return this._name;
}
```

is called with:

```javascript
user.getName();
```

A getter:

```javascript
get name() {
  return this._name;
}
```

is accessed with:

```javascript
user.name;
```

---

## Key Takeaway

- **Getter:** controls what happens when a property is **read**.
- **Setter:** controls what happens when a property is **assigned/changed**.
