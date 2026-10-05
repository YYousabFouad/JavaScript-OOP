## 1. What is a constructor function?

Before `class` syntax existed, JavaScript commonly used **functions as blueprints for creating objects**.

For example:

```
function User(name, age) {
  this.name = name;
  this.age = age;
}
```

This is a **constructor function**.

Its purpose is to create multiple objects that have the same structure.

You can think of it as:

```
User
 │
 ├── name
 └── age
```

Then you can create different users from it:

```
const user1 = new User("Ali", 25);
const user2 = new User("John", 30);
```

Now:

```
user1                 user2
 │                     │
 ├── name: "Ali"       ├── name: "John"
 └── age: 25           └── age: 30
```

Both objects were created from the same constructor function.

---

# 2. Why do we use `this`?

Look at:

```
function User(name, age) {
  this.name = name;
  this.age = age;
}
```

The important question is:

**What does `this` refer to?**

When the function is called with `new`:

```
const user1 = new User("Ali", 25);
```

`this` refers to the **new object being created**.

So conceptually:

```
this.name = name;
```

becomes:

```
user1.name = "Ali";
```

And:

```
this.age = age;
```

becomes:

```
user1.age = 25;
```

---

# 3. What does `new` actually do?

This is the most important part.

When you write:

```
const user1 = new User("Ali", 25);
```

JavaScript performs several steps behind the scenes.

### Step 1 — Create a new empty object

Conceptually:

```
const user1 = {};
```

---

### Step 2 — Connect the object to the constructor's prototype

JavaScript establishes:

```
user1
  │
  ▼
User.prototype
```

This is why constructor functions are strongly connected to **prototypal inheritance/delegation**.

---

### Step 3 — Set `this` to the new object

Inside:

```
function User(name, age) {
  this.name = name;
  this.age = age;
}
```

`this` now points to `user1`.

So the properties are added to `user1`.

---

### Step 4 — Execute the constructor function

The function runs:

```
this.name = name;
this.age = age;
```

Result:

```
user1 = {
  name: "Ali",
  age: 25
};
```

---

### Step 5 — Return the new object

So:

```
const user1 = new User("Ali", 25);
```

gives you the newly created object.

---

# 4. The mental model

Whenever you see:

```
new User("Ali", 25);
```

think:

```
new
 │
 ├── 1. Create empty object
 │
 ├── 2. Link object → User.prototype
 │
 ├── 3. Make `this` point to object
 │
 ├── 4. Execute User(...)
 │
 └── 5. Return object
```

That's the core idea.

---

# 5. Why is the prototype connection important?

Suppose we add a method:

```
User.prototype.sayHello = function () {
  console.log(`Hello ${this.name}`);
};
```

Then:

```
const user1 = new User("Ali", 25);
const user2 = new User("John", 30);
```

Neither object needs to store its own copy of `sayHello`.

Instead:

```
user1 ───────┐
             │
             ▼
       User.prototype
             │
             └── sayHello()

user2 ───────┘
```

When JavaScript evaluates:

```
user1.sayHello();
```

it first looks on `user1`.

If it doesn't find `sayHello`, it delegates the lookup to:

```
User.prototype
```

This is exactly the **prototype delegation** you learned about earlier.

---

# 6. Constructor function vs constructor

Be careful with terminology.

### Constructor function

The function:

```
function User(name, age) {
  this.name = name;
  this.age = age;
}
```

is called a **constructor function** when we use it with `new`.

### `new`

`new` is an **operator** that tells JavaScript:

> Create a new object and use this function to initialize it.

So:

```
new User("Ali", 25);
```

is what gives the function its constructor behavior.

---

# 7. What happens without `new`?

This is important.

If you do:

```
const user1 = User("Ali", 25);
```

you are simply calling the function normally.

You are **not** asking JavaScript to create a new object.

Therefore, `this` does not behave like the newly created object.

This is one of the major reasons `new` matters with constructor functions.

---

## A useful comparison

```
Normal function call

User("Ali", 25)
      │
      ▼
  normal call
```

versus:

```
Constructor call

new User("Ali", 25)
      │
      ▼
  create object
      │
      ▼
  connect prototype
      │
      ▼
  set this
      │
      ▼
  execute function
      │
      ▼
  return object
```

### Key Takeaway

- **Constructor functions** are functions used as blueprints for creating objects.
- **`new`** creates the object, connects it to the constructor's prototype, makes `this` refer to it, runs the constructor, and returns the object.