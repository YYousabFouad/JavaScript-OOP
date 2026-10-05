# ES6 Classes

Before ES6, JavaScript mainly used **constructor functions + prototypes** to create objects and implement inheritance.

ES6 introduced the `class` syntax, which gives us a cleaner way to work with the same underlying prototype system.

The important idea is:

> **A JavaScript class is mostly a cleaner syntax for creating objects and working with prototypes.**

---

## 1. Basic class

Imagine we want to create `Person` objects.

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`My name is ${this.name}`);
  }
}
```

Then:

```javascript
const person1 = new Person("John", 25);
const person2 = new Person("Sarah", 30);
```

Now:

```javascript
console.log(person1.name); // John
console.log(person2.name); // Sarah

person1.introduce();
person2.introduce();
```

---

## 2. What is `constructor()`?

The `constructor()` is a special method inside a class.

It runs **automatically when you use `new`**.

```javascript
const person1 = new Person("John", 25);
```

JavaScript essentially says:

> "Create a new Person object, then run the constructor with these arguments."

So:

```javascript
constructor(name, age) {
  this.name = name;
  this.age = age;
}
```

initializes the newly created object.

### Important

`constructor()` is **not creating the object itself**.

The `new` operator is responsible for creating the object.

The constructor is responsible for **initializing it**.

---

## 3. Where do class methods go?

This is very important considering what we discussed about prototypes.

When you write:

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`My name is ${this.name}`);
  }
}
```

The `introduce()` method is **not copied into every object**.

Instead, it is placed on:

```javascript
Person.prototype
```

So:

```javascript
person1.introduce();
```

works because JavaScript eventually looks at:

```text
person1
   ↓
Person.prototype
   ↓
introduce()
```

This is the **prototype chain**.

---

## 4. Class vs constructor function

Before ES6, you could write:

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.introduce = function () {
  console.log(`My name is ${this.name}`);
};
```

With ES6 classes:

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`My name is ${this.name}`);
  }
}
```

Conceptually:

```text
Constructor Function              Class
───────────────────              ─────

function Person() {}       →     class Person {}

Person.prototype.foo =     →     foo() {}
function () {}
```

The class syntax is much cleaner.

But **JavaScript is still using prototypes underneath**.

---

## 5. Classes are NOT like classes in Java/C++

This is an important JavaScript concept.

You might think:

> "JavaScript classes mean JavaScript stopped using prototypes."

No.

JavaScript still uses:

```text
Objects
   ↓
Prototype
   ↓
Prototype
   ↓
null
```

Classes simply give you a nicer syntax for working with this system.

---

## 6. Class methods

You can define multiple methods:

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`My name is ${this.name}`);
  }

  getAge() {
    return this.age;
  }

  celebrateBirthday() {
    this.age++;
  }
}
```

Then:

```javascript
const person = new Person("John", 25);

person.introduce();
person.celebrateBirthday();

console.log(person.getAge());
```

All these methods are available through `Person.prototype`.

---

## 7. Class inheritance

Classes also provide a clean syntax for inheritance.

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`I am ${this.name}`);
  }
}
```

We can create another class:

```javascript
class Student extends Person {
  study() {
    console.log("I am studying");
  }
}
```

Now:

```javascript
const student = new Student("John");
```

The student can use:

```javascript
student.introduce();
student.study();
```

Why can `student` use `introduce()`?

Because of the prototype chain:

```text
student
   ↓
Student.prototype
   ↓
Person.prototype
   ↓
Object.prototype
   ↓
null
```

This is **prototypal inheritance**, even though we're using class syntax.

---

## 8. `super`

When using inheritance, `super` allows a child class to interact with the parent class.

For example:

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}

class Student extends Person {
  constructor(name, university) {
    super(name);

    this.university = university;
  }
}
```

Here:

```javascript
super(name);
```

calls the **parent constructor**.

So:

```javascript
const student = new Student("John", "Harvard");
```

causes the parent constructor to initialize:

```javascript
this.name
```

and the child constructor initializes:

```javascript
this.university
```

---

## 9. `super` can also call parent methods

Suppose:

```javascript
class Person {
  introduce() {
    console.log("I am a person");
  }
}

class Student extends Person {
  introduce() {
    super.introduce();
    console.log("I am also a student");
  }
}
```

Then:

```javascript
const student = new Student();

student.introduce();
```

The child method can call the parent method through:

```javascript
super.introduce();
```

---

## 10. Class fields

Modern JavaScript also allows properties to be declared directly inside the class.

For example:

```javascript
class Person {
  species = "Human";

  constructor(name) {
    this.name = name;
  }
}
```

Then:

```javascript
const person = new Person("John");

console.log(person.species);
```

Unlike methods, class fields like this are created on **each instance**.

So conceptually:

```text
person
├── name
└── species

Person.prototype
└── methods
```

---

## 11. Static methods

Sometimes a method belongs to the **class itself**, rather than individual objects.

You use `static`.

```javascript
class Person {
  static describe() {
    console.log("A person is a human being");
  }
}
```

You call it like this:

```javascript
Person.describe();
```

Not:

```javascript
const person = new Person();

person.describe(); // ❌
```

Because `describe()` belongs to the class:

```text
Person
   │
   └── static describe()
```

rather than:

```text
Person.prototype
   │
   └── instance methods
```

---

## 12. The big picture

You can think about ES6 classes like this:

```text
                 class Person
                      │
          ┌───────────┴───────────┐
          │                       │
     constructor()          methods
          │                       │
          ↓                       ↓
     Person instance       Person.prototype
          │                       │
          └───────────┬───────────┘
                      │
                prototype chain
```

The most important connection with what we learned earlier is:

```text
ES6 class
   ↓
creates objects with `new`
   ↓
objects delegate to prototypes
   ↓
prototype chain
   ↓
prototypal inheritance
```

So **classes did not replace prototypes**.

They provide a cleaner syntax for using JavaScript's existing prototype-based object system.

### Key Takeaway

- **`class` is cleaner syntax for JavaScript's prototype-based system.**
- **`constructor()` initializes instances, methods live on the class's prototype, and `extends`/`super` build prototype-based inheritance.**
