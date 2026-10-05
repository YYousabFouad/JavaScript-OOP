# JavaScript Object-Oriented Programming (OOP)

A comprehensive, concept-first guide to understanding Object-Oriented Programming (OOP) in JavaScript — from core OOP pillars to JavaScript's prototype delegation model and constructor functions under the hood.

---

## 📚 Table of Contents

1. **[00 - What is OOP?](00-What%20is%20OOP.md)**  
   Introduction to OOP, state (data) and behavior (methods), procedural vs. OOP, classes vs. objects, and communication interfaces (APIs).

2. **[01 - Encapsulation](01-Encapsulation.md)**  
   Bundling data and methods together, protecting internal state, private class fields (`#`), and maintaining clear object boundaries.

3. **[02 - Abstraction](02-Abstraction.md)**  
   Hiding complex internal implementation details and exposing a clean, high-level public interface to callers.

4. **[03 - Inheritance](03-Inheritance.md)**  
   Deriving child classes from parent classes to share and specialize behavior using `extends` and `super()`.

5. **[04 - Polymorphism](04-%20Polymorphism.md)**  
   "One interface, many forms" — implementing and overriding common method interfaces across different subclasses.

6. **[05 - Prototype Property & Prototypal Inheritance](05-%20Prototype%20Property.md)**  
   Deep dive into prototypes: prototype delegation, the prototype chain, `prototype` vs. `__proto__`, property shadowing, built-in prototypes (`Object.prototype`, `Array.prototype`), and memory optimization.

7. **[06 - Constructor Function and The `new` Operator](06-Constructor%20Function%20and%20The%20new%20operator.md)**  
   Classic pre-ES6 object creation: how constructor functions work, the exact 4-step mechanics of the `new` keyword, setting up `this`, and wiring prototype delegation.

8. **[07 - ES6 Classes](07-ES6%20Classes.md)**  
   Modern class syntax: `constructor()`, method definitions on prototypes, class fields, `extends` & `super()`, `static` methods, and how `class` maps to prototype delegation under the hood.

9. **[08 - Setters and Getters](08-Setters%20and%20Getters.md)**  
   Controlled property access: using `get` and `set`, property access syntax vs. method calls, input validation, and protecting internal object state.

---

## 🏛️ The Four Pillars of OOP

### 1. Encapsulation

Keep an object's internal state safe from direct outside manipulation. Expose only explicit methods that validate and protect state.

```javascript
class BankAccount {
  #balance = 0;

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

### 2. Abstraction

Hide complex internal operations behind simple, self-explanatory method names. Consumers interact with what the object does, not how it does it.

```javascript
// Caller doesn't need to know about hashing, salts, or database queries
user.login(credentials);
```

### 3. Inheritance

Reuse code and establish hierarchical relationships where specialized child classes inherit capabilities from general parent classes.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  logout() {
    console.log(`${this.name} logged out.`);
  }
}

class Admin extends User {
  deleteDatabase() {
    console.log("Database deleted.");
  }
}
```

### 4. Polymorphism

Different objects can implement the same method interface in distinct, specialized ways.

```javascript
class Dog {
  speak() {
    return "Woof!";
  }
}

class Cat {
  speak() {
    return "Meow!";
  }
}

function playSound(animal) {
  console.log(animal.speak());
}
```

---

## ⚙️ JavaScript Under the Hood: Delegation, Not Copying

Unlike classical class-based languages (like Java or C++), JavaScript implements OOP through **prototypes and delegation**:

```text
[instance]  ──(__proto__)──>  [Constructor.prototype]  ──(__proto__)──>  [Object.prototype]  ──>  null
```

### Key Concepts

| Concept | Explanation |
| :--- | :--- |
| **Prototype Delegation** | When accessing `obj.prop`, if `obj` doesn't have it, JavaScript searches its prototype link (`__proto__`) rather than copying properties onto every instance. |
| **`Constructor.prototype`** | An object blueprint attached to a function/class where shared methods are defined once in memory. |
| **`instance.__proto__`** | An internal reference on every object pointing to the prototype object it delegates to. |
| **The `new` Operator** | Creates a blank object, sets its `__proto__` to the constructor's `prototype`, binds `this`, and returns the newly created object. |
| **ES6 `class` Syntax** | Clean, declarative syntactic sugar over constructor functions and prototype delegation — the underlying engine remains prototypal. |

---

## 💡 Quick Rules of Thumb

- **Put instance-specific state in the constructor** (`this.name = name`).
- **Put shared methods on the prototype** (`User.prototype.login = ...` or inside the `class` body) so all instances share a single function reference in memory.
- **Never mutate `Object.prototype` directly** in production code.
