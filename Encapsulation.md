
**Encapsulation** means:

> **Keeping an object's data and the methods that work with that data together, while controlling how that data can be accessed or changed.**

Think of an object as a **box**:

```
┌─────────────────────────┐
│        Object           │
│                         │
│  Data                   │
│  ├── name               │
│  └── balance            │
│                         │
│  Methods                │
│  ├── deposit()          │
│  └── withdraw()         │
└─────────────────────────┘
```

The important idea is that the object can decide **what outside code is allowed to do with its data**.

### Without encapsulation

Imagine:

```
const account = {
  balance: 1000
};

account.balance = -5000;
```

Nothing prevents outside code from directly changing `balance`.

### With encapsulation

A class can make a property **private** using `#`:

```
class Account {
  #balance = 1000;

  deposit(amount) {
    this.#balance += amount;
  }

  getBalance() {
    return this.#balance;
  }
}
```

Now:

```
const account = new Account();

account.deposit(500);

console.log(account.getBalance());
```

Outside code **cannot directly access**:

```
account.#balance; // ❌
```

Instead, it interacts with the object through its methods.

### The key idea

Encapsulation is **not simply "making properties private."**

It is about creating a controlled boundary:

```
Outside code
     │
     │ allowed interaction
     ▼
┌───────────────┐
│     Object    │
│               │
│  #private     │ ← protected data
│               │
│  methods()    │ ← controlled access
└───────────────┘
```

So when you see:

```
class User {
  #password;

  login(password) {
    // ...
  }
}
```

The idea is:

> "The object owns and controls its internal data. Other code should interact with it through the interface I provide."

### Key Takeaway

- **Encapsulation = protecting an object's internal state and controlling how outside code interacts with it.**
- JavaScript supports this with mechanisms such as **private fields (`#`)**, methods, and controlled interfaces.