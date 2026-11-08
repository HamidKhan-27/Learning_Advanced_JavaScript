# JavaScript Advanced Topics

## Topic #13 — Closures: Complete Study Notes

### Topics Covered

1. Scope: Global, Function, and Block Scope
2. Outer and Inner Functions
3. What Is a Closure?
4. Returning Functions
5. Closures and Persistent Variables
6. Private Variables and Data Encapsulation
7. Independent Closures
8. Closures with Objects
9. Real-World Applications
10. Important Differences and Practice

---

## 1. What Is Scope?

**Scope** determines where a variable can be accessed in JavaScript.

### Global Scope

A variable declared outside a function can generally be accessed throughout the script.

```javascript
const username = "Hamid";

function greet() {
  console.log(username);
}

greet(); // Hamid
console.log(username); // Hamid
```

### Function Scope

A variable declared inside a function is accessible within that function and its nested functions, but not directly outside it.

```javascript
function example() {
  const message = "Hello";
  console.log(message); // Hello
}

example();

// console.log(message); // ReferenceError
```

### Block Scope

Variables declared with `let` and `const` inside a block `{}` are limited to that block.

```javascript
if (true) {
  let age = 20;
  const city = "Lahore";

  console.log(age); // 20
  console.log(city); // Lahore
}

// console.log(age); // ReferenceError
```

**Important:** Closures depend on lexical scope: the scope determined by where functions are written in the code.

---

## 2. Outer and Inner Functions

An inner function is a function defined inside another function.

```javascript
function outer() {
  const name = "Ali";

  function inner() {
    console.log(name);
  }

  inner();
}

outer(); // Ali
```

The inner function can access `name` because it belongs to the surrounding scope.

However, the outer function cannot automatically access variables declared only inside the inner function.

---

## 3. What Is a Closure?

**A closure is created when a function retains access to variables from its surrounding lexical scope, even after the outer function has finished executing.**

```javascript
function outer() {
  const username = "Hamid";

  function inner() {
    console.log(username);
  }

  return inner;
}

const myFunction = outer();

myFunction(); // Hamid
```

### How does this work?

1. `outer()` starts executing.
2. It creates the variable `username`.
3. It returns the `inner` function.
4. `outer()` finishes executing.
5. `myFunction()` executes the returned function.
6. The inner function can still access `username`, so it prints `"Hamid"`.

**Key point:** The closure preserves access to the necessary surrounding variables. It does not mean every outer variable becomes globally accessible.

---

## 4. `return inner` vs `return inner()`

This is one of the most important concepts in closures.

### Returning a function reference

```javascript
function outer() {
  function inner() {
    return 25;
  }

  return inner;
}

const result = outer();

console.log(result); // Function reference
console.log(result()); // 25
```

`return inner` returns the function itself. It does not execute `inner` at that moment.

### Returning the function's result

```javascript
function outer() {
  function inner() {
    return 25;
  }

  return inner();
}

const result = outer();

console.log(result); // 25
```

Here, `inner()` executes immediately, and `outer()` returns its result.

| Code             | Meaning                                    |
| ---------------- | ------------------------------------------ |
| `return inner`   | Return the function reference              |
| `return inner()` | Execute the function and return its result |

---

## 5. Closures Preserve Variable State

A closure can remember and update a variable between function calls.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

### Why does the count increase?

- `createCounter()` creates `count = 0`.
- The returned function closes over `count`.
- Each call increments the same variable.
- The variable's updated value remains available to that closure.

**Important:** The closure does not simply copy the initial value `0`. It retains access to the variable and its changing state.

---

## 6. Private Variables

Closures allow you to keep data inaccessible through direct external access.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2

// console.log(count); // ReferenceError
```

The variable `count` is local to `createCounter()`. It cannot be accessed directly from outside.

However, the returned function can still read and update it.

This is a basic form of **data encapsulation**.

---

## 7. Independent Closures

Every call to `createCounter()` creates a new local `count` variable.

```javascript
function createCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // 1
console.log(counter1()); // 2

console.log(counter2()); // 1
console.log(counter1()); // 3
console.log(counter2()); // 2
```

### Remember

- `counter1` maintains its own count.
- `counter2` maintains a separate count.
- Calling one counter does not change the other's count.

**Common mistake:** Assuming that all functions created by the same function share one variable. They do not share it when each invocation creates a separate local variable.

---

## 8. Closures with Objects

Closures can also keep data private while exposing selected operations through an object.

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function () {
      count++;
    },

    getCount: function () {
      return count;
    },
  };
}

const counter = createCounter();

counter.increment();
counter.increment();

console.log(counter.getCount()); // 2
console.log(counter.count); // undefined
```

### Why is the second result `undefined`?

The object has two methods: `increment` and `getCount`. It does not have a public `count` property.

The methods retain access to the local `count` variable through closures, but `counter.count` attempts to read a property on the object itself.

---

## 9. Real-World Applications

### A. Counters

Useful for tracking clicks, attempts, or repeated actions.

```javascript
function createClickCounter() {
  let clicks = 0;

  return function () {
    clicks++;
    console.log("Clicks:", clicks);
  };
}

const countClicks = createClickCounter();

countClicks(); // Clicks: 1
countClicks(); // Clicks: 2
```

### B. Private settings

```javascript
function createAccount() {
  let balance = 1000;

  return {
    deposit: function (amount) {
      balance += amount;
    },

    getBalance: function () {
      return balance;
    },
  };
}

const account = createAccount();

account.deposit(500);

console.log(account.getBalance()); // 1500
```

The balance can be changed through the provided methods without exposing a direct public `balance` property.

### C. Event handlers

Closures allow event-handler functions to access variables from the surrounding scope.

```javascript
function setupButton() {
  let clicks = 0;

  document.getElementById("myButton").addEventListener("click", function () {
    clicks++;
    console.log("Button clicked:", clicks);
  });
}

setupButton();
```

Each click updates the same `clicks` variable retained by the event handler's closure.

HTML:

```html
<button id="myButton">Click Me</button>
```

---

## 10. Important Points to Remember

1. A closure retains access to variables from its surrounding lexical scope.
2. An inner function can access variables declared in its outer function.
3. `return inner` returns a function reference; `return inner()` executes the function.
4. Closures can preserve changing variable state between calls.
5. Each invocation of a factory function can create an independent closure.
6. Local variables are not automatically accessible outside their scope.
7. Closures can provide private state through returned functions or objects.
8. `undefined` and `ReferenceError` are different: a missing object property usually produces `undefined`, while accessing an undeclared identifier produces a `ReferenceError`.
9. Closures are widely used in callbacks, event handlers, counters, and data encapsulation.

---

## Final Practice

### Question 1

What is the output?

```javascript
function outer() {
  let x = 5;

  return function () {
    x += 2;
    return x;
  };
}

const fn = outer();

console.log(fn());
console.log(fn());
```

**Answer:** `7`, then `9`.

### Question 2

What is the output?

```javascript
function make() {
  let value = 10;

  return function () {
    return value;
  };
}

const fn = make();

console.log(fn());
console.log(value);
```

**Answer:** The first call returns `10`. The second statement throws a `ReferenceError` because `value` is not accessible directly outside `make()`.

### Question 3

What is the output?

```javascript
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const a = counter();
const b = counter();

console.log(a());
console.log(b());
console.log(a());
```

**Answer:** `1`, `1`, `2`. Each counter has its own independent state.

---

## Topic #13 Complete

**Next topic: #14 — Object-Oriented Programming (OOP) with JavaScript.**

We will cover objects, constructors, prototypes, classes, inheritance, encapsulation, and practical examples before moving to the tests.
