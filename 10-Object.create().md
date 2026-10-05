# Object.create()

`Object.create()` is one of the clearest ways to understand **prototypal inheritance** in JavaScript.

The key idea is:

> **`Object.create()` creates a new object whose prototype is the object you give it.**

Let's build the idea step by step.

---

## 1. The basic idea

Suppose we have:

```javascript
const userMethods = {
  login() {
    console.log("User logged in");
  },

  logout() {
    console.log("User logged out");
  },
};
```

Now we create another object:

```javascript
const user1 = Object.create(userMethods);
```

Conceptually:

```text
user1
  │
  │ [[Prototype]]
  ▼
userMethods
  │
  ├── login()
  └── logout()
```

`user1` **does not contain** `login()` or `logout()` as its own properties.

Instead, JavaScript can find them through the prototype.

```javascript
user1.login();
```

JavaScript looks for `login`:

```text
Does user1 have login?
        ↓
       No
        ↓
Does user1's prototype have login?
        ↓
       Yes
        ↓
Run it
```

This is **prototype delegation**.

---

## 2. What does `Object.create()` actually do?

The syntax is:

```javascript
Object.create(prototype);
```

For example:

```javascript
const person = {
  sayHello() {
    console.log("Hello");
  },
};

const john = Object.create(person);
```

This means:

> Create a new object and make `person` its prototype.

So:

```javascript
Object.getPrototypeOf(john) === person
```

is:

```javascript
true
```

You can think of it as:

```text
john
 ↓
person
 ↓
Object.prototype
 ↓
null
```

---

## 3. Why is this useful for OOP?

OOP is largely about creating **objects that have data and behavior**.

For example, imagine users:

```text
User
├── name
├── email
├── login()
└── logout()
```

We don't want every user object to contain separate copies of the same methods.

Instead, we can put shared behavior into a prototype.

```text
            userMethods
          ┌──────────────┐
          │ login()      │
          │ logout()     │
          └──────┬───────┘
                 │
        ┌────────┴────────┐
        ↓                 ↓
      user1              user2
   name: "Ali"        name: "John"
   email: ...         email: ...
```

Both objects delegate their methods to the same object.

---

## 4. Adding object-specific data

We can give `user1` its own properties:

```javascript
const user1 = Object.create(userMethods);

user1.name = "Ali";
user1.email = "ali@example.com";
```

Now:

```text
user1
├── name
├── email
│
└── [[Prototype]]
       ↓
   userMethods
   ├── login()
   └── logout()
```

So:

```javascript
console.log(user1.name);
```

finds `name` directly on `user1`.

But:

```javascript
user1.login();
```

finds `login` on the prototype.

---

## 5. Own properties vs inherited properties

This distinction is **very important**.

```javascript
const userMethods = {
  login() {
    console.log("Logged in");
  },
};

const user1 = Object.create(userMethods);

user1.name = "Ali";
```

`name` is an **own property**:

```javascript
user1.hasOwnProperty("name");
```

→ `true`

But `login` is inherited:

```javascript
user1.hasOwnProperty("login");
```

→ `false`

Yet:

```javascript
user1.login();
```

still works.

Why?

Because JavaScript searches the prototype chain.

---

## 6. The prototype chain

Suppose:

```javascript
const userMethods = {
  login() {
    console.log("Logged in");
  },
};

const user1 = Object.create(userMethods);
```

The chain looks roughly like:

```text
user1
  │
  │ [[Prototype]]
  ▼
userMethods
  │
  │ [[Prototype]]
  ▼
Object.prototype
  │
  │ [[Prototype]]
  ▼
null
```

Now imagine:

```javascript
user1.toString();
```

Does `user1` have `toString`?

No.

Does `userMethods` have it?

No.

Does `Object.prototype` have it?

Yes.

So JavaScript finds it there.

This is the **prototype chain**.

---

## 7. `Object.create()` with properties

There is another form:

```javascript
Object.create(prototype, properties);
```

For example:

```javascript
const user1 = Object.create(userMethods, {
  name: {
    value: "Ali",
  },
});
```

Now `name` is created as a property of `user1`.

However, as a beginner, I recommend focusing first on:

```javascript
Object.create(prototype)
```

because the second argument involves **property descriptors**, which is a separate concept.

---

## 8. A more OOP-like example

Let's imagine a `Person`.

We could create a prototype object:

```javascript
const personPrototype = {
  introduce() {
    console.log(`Hi, I am ${this.name}`);
  },

  walk() {
    console.log(`${this.name} is walking`);
  },
};
```

Then:

```javascript
const person1 = Object.create(personPrototype);
const person2 = Object.create(personPrototype);
```

Give each person their own data:

```javascript
person1.name = "Ali";
person2.name = "John";
```

Now:

```javascript
person1.introduce();
```

and:

```javascript
person2.introduce();
```

Both use the same `introduce()` method.

But `this` changes.

For `person1`:

```text
this → person1
```

For `person2`:

```text
this → person2
```

So the shared method can work with different objects.

---

## 9. Why don't we just put the methods directly on each object?

We could do this:

```javascript
const person1 = {
  name: "Ali",

  introduce() {
    console.log(`Hi, I am ${this.name}`);
  },
};

const person2 = {
  name: "John",

  introduce() {
    console.log(`Hi, I am ${this.name}`);
  },
};
```

But now there are two separate `introduce` function properties.

Conceptually:

```text
person1 → introduce() ← separate function

person2 → introduce() ← another separate function
```

With a prototype:

```text
             personPrototype
                  │
             introduce()
              ↙       ↘
          person1     person2
```

Both delegate to the same method.

That's one of the important benefits of prototypes.

---

## 10. `Object.create()` vs `new`

This is where it becomes especially interesting.

You previously learned about constructor functions.

For example, conceptually:

```javascript
new Person()
```

creates an object and connects it to:

```javascript
Person.prototype
```

With `Object.create()`, **you explicitly choose the prototype object**.

```javascript
const person1 = Object.create(personPrototype);
```

So:

### Constructor approach

```text
new Person()
      ↓
new object
      ↓
Person.prototype
```

### Object.create approach

```text
Object.create(personPrototype)
            ↓
       new object
            ↓
      personPrototype
```

The underlying idea is very similar:

> **Create an object and connect it to another object through the prototype.**

---

## 11. `Object.create()` does NOT call a constructor

This is an important difference.

With:

```javascript
new Person();
```

JavaScript invokes the constructor function.

With:

```javascript
Object.create(personPrototype);
```

JavaScript does **not** call anything.

It simply creates an object with the specified prototype.

Think:

```text
Object.create()
     │
     ├── creates object
     │
     └── sets [[Prototype]]
```

While `new` involves additional behavior:

```text
new Person()
     │
     ├── creates object
     ├── connects it to Person.prototype
     ├── calls Person with this
     └── returns the object
```

---

## 12. `Object.create(null)`

There is one particularly interesting use:

```javascript
const obj = Object.create(null);
```

This creates an object with **no prototype**.

Normally:

```text
object
  ↓
Object.prototype
```

But here:

```text
object
  ↓
null
```

So it doesn't inherit methods such as:

```javascript
toString()
hasOwnProperty()
```

This is sometimes useful when you want a very "pure" dictionary-like object.

But it is not something you need to use often as a beginner.

---

## 13. The most important mental model

Don't think:

> "`Object.create()` copies the object."

It does **not** copy it.

This is a very common misunderstanding.

Suppose:

```javascript
const prototype = {
  sayHello() {
    console.log("Hello");
  },
};

const obj = Object.create(prototype);
```

`obj` does not contain a copy of `sayHello`.

Instead:

```text
obj
 │
 │ "I don't have sayHello."
 │
 ▼
prototype
 │
 └── sayHello()
```

It **delegates** the lookup to the prototype.

---

## 14. One final picture

The whole concept can be visualized like this:

```text
             ┌─────────────────────┐
             │   personPrototype   │
             │                     │
             │ introduce()         │
             │ walk()              │
             └──────────┬──────────┘
                        │
             [[Prototype]]
                 ↙           ↘
                ↓             ↓
        ┌──────────────┐ ┌──────────────┐
        │   person1    │ │   person2    │
        │              │ │              │
        │ name: "Ali"  │ │ name: "John" │
        └──────────────┘ └──────────────┘
```

`person1` and `person2` own their **data**, while the prototype provides their **shared behavior**.

---

## Key Takeaway

- **`Object.create(proto)` creates a new object whose `[[Prototype]]` is `proto`.**
- It is a direct way to implement **prototypal inheritance/delegation**: the object looks up missing properties in its prototype chain.
