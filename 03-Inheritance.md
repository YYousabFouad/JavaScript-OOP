# Inheritance

**Inheritance** is an object-oriented programming idea that lets one kind of object reuse and extend behavior associated with another kind of object.

Think of it as:

> **Child → gets shared features from → Parent**

For example, a `Student` is a kind of `Person`. A person may have a name and a way to say hello; a student has those shared features and can also have student-specific behavior, such as studying. This relationship helps avoid describing the same shared behavior separately for every kind of person.

Inheritance does not mean that JavaScript copies every line from the parent into each child object. In JavaScript, objects are linked through **prototypes**. When a property or method is not found directly on an object, JavaScript follows those links to look for it.

## A First Mental Model

Imagine asking a student object, “Can you say hello?” JavaScript checks in this order:

1. Does this particular student object have `sayHello`?
2. Does the student’s shared prototype have it?
3. Does the parent person’s shared prototype have it?
4. Continue up the chain until the property is found or the chain ends.

The first matching property is used. If JavaScript reaches the end without finding it, the result is `undefined` (or a method call fails because there is no function to call).

```text
student object
      ↓
Student.prototype
      ↓
Person.prototype
      ↓
Object.prototype
      ↓
null (end of the chain)
```

The object’s own data, such as a particular student's name, is usually stored on the object itself. Methods meant to be shared are usually found on a prototype. This lets many instances use the same method rather than each carrying its own separate copy.

## 1. Inheritance with Constructor Functions

Before ES6 classes were introduced, JavaScript commonly used **constructor functions** to create objects. A constructor function is an ordinary function intended to be called with `new`. Inside it, `this` refers to the new object being created, so assigning a name to `this` gives each new person their own name.

To create an inheritance relationship with constructor functions, two separate links are needed:

- **Instance setup:** the child constructor calls the parent constructor so the new child instance receives the parent’s own data, such as its name.
- **Method lookup:** the child constructor’s prototype is linked to the parent constructor’s prototype so child instances can find the parent’s shared methods.

These links solve different problems. Calling the parent constructor sets up per-instance properties. Linking prototypes makes shared methods available. Doing only one of these steps does not provide both parts of the relationship.

### Example in words

Suppose the `Person` constructor records a name on each new object, and a shared `Person` method can greet. A `Student` constructor first uses the `Person` setup for the new student, then records the student’s grade. The student’s prototype is connected to the person’s prototype, and `Student` can add a shared `study` method of its own.

When a student asks for `study`, JavaScript finds it on `Student.prototype`. When the student asks for the greeting method, JavaScript checks `Student.prototype`, does not find it there, then finds it on `Person.prototype`.

### Prototype chain

```text
student instance
  own data: name, grade
      ↓
Student.prototype
  shared behavior: study
      ↓
Person.prototype
  shared behavior: sayHello
      ↓
Object.prototype
      ↓
null
```

### A common point of confusion

The constructor function and its `.prototype` property are not the same thing. The constructor function is used to make instances. Its `.prototype` is an object that instances created by that constructor can consult through their prototype chain.

Also, the special `prototype` property belongs to constructor functions, while an instance’s internal prototype link points to the object used during property lookup. They are connected concepts, but not identical properties.

## 2. Inheritance with ES6 Classes

ES6 classes provide a clearer way to write constructor-based object creation and inheritance. A class can describe how each instance is initialized, and `extends` expresses that one class is based on another.

Using the existing example, `Student extends Person` says that `Student` is the child class and `Person` is the parent class. A student instance can use student behavior and, through the prototype chain, person behavior.

### The `constructor` and `super`

The class `constructor` is the setup step that runs when a new instance is created. If the child class defines its own constructor, it must call `super(...)` before it uses `this`. In this setting, `super(...)` runs the parent class’s constructor so the inherited part of the instance is initialized.

For example, a `Student` may pass its name to the `Person` constructor and then store its grade. The resulting instance has both pieces of data: its name from the parent setup and its grade from the child setup.

`super` can also be used inside a child method to call a method from the parent class. It is a way to refer to the parent implementation in the current class relationship; it does not create a separate parent object.

### What classes do under the hood

The class syntax is more readable, but JavaScript still uses prototypes for method lookup. In a typical class inheritance relationship, there are two related chains:

```text
Instance method lookup:
student instance → Student.prototype → Person.prototype → Object.prototype → null

Class-level lookup:
Student class → Person class → Function.prototype → Object.prototype → null
```

The first chain is the important one when an instance looks for methods such as `study` or `sayHello`. The second chain relates to properties looked up on the class constructors themselves, including inherited static members.

Classes do not make JavaScript inheritance work like classical languages in every respect. They are syntax built on JavaScript’s prototype system, and class methods are placed on the class’s prototype rather than copied onto each instance.

## 3. Inheritance with `Object.create()`

`Object.create()` starts with a different emphasis: it creates a new object whose prototype is the object you provide. It does not require a constructor function or an ES6 class.

If a `person` object contains shared behavior, an object created with `Object.create(person)` uses `person` as its prototype. If a requested property is not on the new object itself, JavaScript checks `person`, then follows the rest of `person`’s prototype chain.

### Object.create() example in words

Think of a general person object that provides a greeting method. Create a student object whose prototype is that person object. Add a name and grade directly to the student object. The student can use its own name and grade, and it can find the greeting by following its prototype link to the person object.

```text
student object
  own data: name, grade
      ↓
person object
  shared behavior: sayHello
      ↓
Object.prototype
      ↓
null
```

This illustrates **delegation**: the student object delegates a missing-property lookup to its prototype. `Object.create()` establishes the prototype link, but it does not automatically run a parent constructor or initialize fields. Any needed own data must be set up separately.

If `Object.create(null)` is used, the new object has no prototype at all. That can be useful for special dictionary-like objects, but it also means the object does not inherit methods from `Object.prototype`.

## Comparing the Three Approaches

| Approach | How the relationship is expressed | How parent setup happens | Where shared methods are found |
| --- | --- | --- | --- |
| Constructor functions | Connect the child constructor’s prototype to the parent constructor’s prototype | The child constructor calls the parent constructor with the new instance as its context | On the linked prototypes |
| ES6 classes | Use `extends` between classes | The child constructor calls `super(...)` when it defines its own constructor | On `Child.prototype`, then `Parent.prototype` |
| `Object.create()` | Create an object with another object as its prototype | No constructor runs automatically; setup is separate | On the chosen prototype object and above it |

All three approaches rely on the **prototype chain** for inherited property and method lookup. The main difference is how they create objects and express the links:

- Constructor functions show the older, explicit setup pattern.
- ES6 classes provide familiar class-oriented syntax while still using prototypes.
- `Object.create()` focuses directly on linking one object to another.

## Own Properties and Inherited Properties

An **own property** belongs directly to one object. For instance, two student objects can each have their own name and grade.

An **inherited property** is found somewhere above the object in its prototype chain. A shared greeting method can live on a prototype and be available to many student objects.

If an object gets its own property with the same name as an inherited property, the own property is found first. This is called **shadowing**: the inherited property still exists, but lookup stops at the nearer own property. Removing the own property can reveal the inherited one again.

## Why Inheritance Is Useful—and When to Be Careful

Inheritance is useful when there is a genuine “is a kind of” relationship and several kinds of objects should share behavior. A student is a kind of person, so sharing person behavior is a natural fit.

It can become confusing when a child depends on many details of its parent, or when the relationship is only a superficial similarity. Changes higher in a prototype chain can affect every object that depends on it. For unrelated objects that merely need the same capability, sharing a small helper or composing objects can sometimes be easier to understand.

## Key Takeaway

- JavaScript inheritance works through **prototype links**: when a property is missing on an object, JavaScript searches up its prototype chain.
- Constructor functions, ES6 classes, and `Object.create()` express object creation and prototype links differently, but they all rely on that same lookup behavior.
