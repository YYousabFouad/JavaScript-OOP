# Polymorphism

**Polymorphism** means:

> **“One interface, different behaviors.”**

The word comes from:

- **Poly** = many
- **Morph** = forms

In OOP, polymorphism means that **different objects can respond to the same
method in different ways**.

## Simple Example

Imagine we have a `Animal` class:

```javascript
class Animal {
  speak() {
    console.log("The animal makes a sound");
  }
}
```

Now different animals can have their own version of `speak()`:

```javascript
class Dog extends Animal {
  speak() {
    console.log("Woof!");
  }
}

class Cat extends Animal {
  speak() {
    console.log("Meow!");
  }
}
```

Now:

```javascript
const dog = new Dog();
const cat = new Cat();

dog.speak(); // Woof!
cat.speak(); // Meow!
```

Notice something important:

Both objects use the **same method name**:

```javascript
speak()
```

But they produce **different behavior**.

That's polymorphism.

## The Important Idea

You don't need to care about the exact type of object when calling the method:

```javascript
animal.speak();
```

The object itself determines **which version of `speak()` runs**.

So you can think of it like:

```text
             speak()
                │
       ┌────────┴────────┐
       ↓                 ↓
      Dog               Cat
       │                 │
    "Woof!"            "Meow!"
```

This is one of the main benefits of polymorphism: **you can write code that
works with different objects through a common interface.**

## Key Takeaway

- **Polymorphism = same interface/method, different behavior.**
- In JavaScript, inheritance + method overriding is a common way to achieve
  polymorphism.
