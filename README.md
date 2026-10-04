# JavaScript Object-Oriented Programming (OOP)

A clear, concept-first guide to understanding Object-Oriented Programming (OOP)
in JavaScript.

This folder breaks down OOP fundamentals, the four core pillars, and how
JavaScript implements OOP under the hood using prototypes and delegation.

---

## Table of Contents

1. **[What is OOP?](00-What%20is%20OOP.md)**  
   Introduction to OOP, classes vs. objects, and communication interfaces
   (APIs).

2. **[Encapsulation](01-Encapsulation.md)**  
   Bundling data & behavior, private fields (`#`), and controlled boundaries.

3. **[Abstraction](02-Abstraction.md)**  
   Hiding internal complexity and exposing clean public interfaces.

4. **[Inheritance](03-Inheritance.md)**  
   Sharing and extending behavior using `extends` and `super()`.

5. **[Polymorphism](04-%20Polymorphism.md)**  
   "One interface, different behaviors" through method overriding.

6. **[Prototypal Inheritance](05-%20Prototypal%20Inheritance%20%28Prototype%20Delegation%29.md)**  
   The prototype chain, property shadowing, and object delegation.

---

## Core Pillars at a Glance

### 1. Encapsulation

Keep an object's internal state protected and only allow interaction through
defined methods.

```javascript
class Account {
  #balance = 0;

  deposit(amount) {
    this.#balance += amount;
  }
}
```

### 2. Abstraction

Hide implementation details and expose only what the consumer needs to know.

```javascript
// Callers use a simple method without needing to know internal logic
user.login();
```

### 3. Inheritance

Derive a child class from a parent class to share and specialize behavior.

```javascript
class Student extends Person {
  study() {
    console.log("Studying...");
  }
}
```

### 4. Polymorphism

Different classes can respond to the exact same method in their own unique way.

```javascript
dog.speak(); // "Woof!"
cat.speak(); // "Meow!"
```

---

## JavaScript Mental Model: Delegation, Not Copying

Unlike class-based languages (such as Java or C++), JavaScript uses
**prototypal inheritance**:

- Objects do not copy properties from parent classes.
- When an object doesn't own a property, JavaScript **delegates** the lookup up
  the **prototype chain**.
- `class` syntax in modern JavaScript is ergonomic syntax built on top of this
  prototype delegation model.
