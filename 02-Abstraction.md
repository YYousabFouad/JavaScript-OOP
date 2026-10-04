# Abstraction

**Abstraction** means:

> **Hide the complicated internal details and expose only what is necessary
> to use something.**

Think of a **car**:

You use:

```text
Start the car
    ↓
Press the button
    ↓
Engine starts
```

You don't need to know everything happening inside the engine.

That's abstraction.

---

## In JavaScript

For example:

```javascript
class User {
  #password;

  constructor(password) {
    this.#password = password;
  }

  login() {
    return this.#checkPassword();
  }

  #checkPassword() {
    // complicated password checking...
    return true;
  }
}
```

Someone using this class only needs:

```javascript
const user = new User("1234");

user.login();
```

They don't need to know **how** `#checkPassword()` works.

So we have:

```text
Outside
   │
   │  user.login()
   ▼
┌─────────────────────┐
│       User          │
│                     │
│  login()            │ ← exposed
│      ↓              │
│  #checkPassword()   │ ← hidden
└─────────────────────┘
```

## The Important Idea

Abstraction focuses on:

- **"What can I use?"** (the public interface)

rather than:

- **"How does it work internally?"** (the implementation details)

For example, when you use:

```javascript
array.map(...)
```

you don't need to understand the internal algorithm JavaScript uses to
implement `map()`.

You simply know:

> "`map()` transforms the elements of an array and gives me a new array."

That is abstraction.

## Abstraction vs Encapsulation

They are related, but not identical:

- **Encapsulation** → controls access to data/implementation.
- **Abstraction** → hides unnecessary complexity and gives you a simpler
  interface.

You can think of it as:

```text
Encapsulation
     ↓
Hide/protect internal parts
     ↓
Abstraction
     ↓
Expose only what the user needs
```

## Key Takeaway

- **Abstraction = hide complexity, expose the essential interface.**
- Ask yourself: **"Can I use this thing without knowing how it works
  internally?"** If yes, abstraction is involved.
