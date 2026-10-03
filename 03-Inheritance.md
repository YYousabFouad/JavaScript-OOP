
**Inheritance** is an OOP concept where one class can **reuse properties and methods from another class**.

Think of it as:

> **Child class → gets features from → Parent class**

### Simple example

Imagine we have a general `Person`:

```
class Person {
  constructor(name) {
    this.name = name;
  }

  sayHello() {
    console.log(`Hello, I'm ${this.name}`);
  }
}
```

Now we create a `Student`:

```
class Student extends Person {
  study() {
    console.log("I'm studying");
  }
}
```

The important part is:

```
extends Person
```

It means:

> `Student` inherits from `Person`.

So a `Student` object can use **both** its own methods and the inherited ones:

```
const student = new Student("Ahmed");

student.sayHello(); // inherited from Person
student.study();    // Student's own method
```

### What actually happened?

`Student` didn't define `sayHello()` itself.

But because it **extends `Person`**, JavaScript allows a `Student` object to access methods defined in `Person`.

```
Person
 ├── name
 └── sayHello()
       ↑
       │ inherited
       │
Student
 └── study()
```

### `super`

Inheritance becomes especially useful when the child needs to add to the parent's constructor.

```
class Student extends Person {
  constructor(name, grade) {
    super(name);
    this.grade = grade;
  }
}
```

`super(name)` calls the **parent's constructor**.

So:

```
const student = new Student("Ahmed", 10);
```

results in the student having:

```
name  → "Ahmed"   ← from Person
grade → 10        ← from Student
```

### One important idea

Inheritance is not simply "copying code."

The child maintains a relationship with the parent through JavaScript's **prototype chain**.

Conceptually:

```
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

When you do:

```
student.sayHello();
```

JavaScript looks for `sayHello()` on `student`, then `Student.prototype`, then `Person.prototype`.

---

### Key Takeaway

- **Inheritance** allows a child class to reuse and extend behavior from a parent class.
- `extends` creates the inheritance relationship, while `super()` allows the child to use the parent's constructor or methods.