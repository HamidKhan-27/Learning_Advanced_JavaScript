# Topic #15 — Asynchronous JavaScript (Part 1)

## Asynchronous JavaScript Fundamentals

### Topics Covered

1. Synchronous JavaScript
2. Asynchronous JavaScript
3. JavaScript Runtime
4. Call Stack
5. Web APIs
6. Callback Queue
7. Event Loop
8. Microtask Queue
9. setTimeout()
10. setInterval()
11. Callback Functions
12. Callback Hell

---

## 1. Synchronous JavaScript

JavaScript normally executes code synchronously. This means that statements are executed one by one, in the order they appear.

A statement must finish before JavaScript moves to the next statement.

### Example

```javascript
console.log("First");
console.log("Second");
console.log("Third");
```

### Output

```text
First
Second
Third
```

Each statement executes before the next one.

### Blocking Code

A long-running operation can block the execution of other JavaScript code.

```javascript
console.log("Start");

for (let i = 1; i <= 1000000000; i++) {
  // Long-running operation
}

console.log("End");
```

JavaScript cannot execute `console.log("End")` until the loop finishes.
This is called blocking the main thread.

---

## 2. Asynchronous JavaScript

Asynchronous JavaScript allows certain operations to start and finish later without blocking the remaining code.

For example:

- Timers
- Network requests
- Fetching data from a server
- Event handling

### Example

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Middle");
}, 2000);

console.log("End");
```

### Output

```text
Start
End
Middle
```

### Explanation

1. `"Start"` is printed immediately.
2. `setTimeout()` schedules its callback.
3. JavaScript continues executing the next statement.
4. `"End"` is printed.
5. After the timer is ready, the callback can execute.

**Important:** JavaScript does not stop and wait for two seconds. The timer is handled by the runtime environment.

---

## 3. JavaScript Runtime

JavaScript Runtime is the environment in which JavaScript code executes.

Examples include:

- Browser
- Node.js

A JavaScript runtime provides more than just the JavaScript engine. It also provides facilities such as timers, networking and other APIs.

### JavaScript Engine vs Runtime

| JavaScript Engine        | JavaScript Runtime                            |
| ------------------------ | --------------------------------------------- |
| Executes JavaScript code | Provides the complete execution environment   |
| Handles code execution   | Includes the engine and additional facilities |
| Example: V8              | Example: Browser or Node.js                   |

Chrome uses the V8 JavaScript engine.

---

## 4. Call Stack

The Call Stack keeps track of function calls during execution.
It follows the **LIFO** principle:
Last In, First Out.
The last function added to the stack is the first one to finish.

### Example

```javascript
function first() {
  console.log("First");
}

function second() {
  first();
  console.log("Second");
}

second();
```

### Output

```text
First
Second
```

### How It Works

1. `second()` is called.
2. `second()` enters the Call Stack.
3. `first()` is called and enters the stack.
4. `first()` finishes and leaves the stack.
5. `second()` continues and finishes.

The Call Stack manages function execution. It does not store all future asynchronous callbacks.

---

## 5. Web APIs

Web APIs are features provided by the browser environment that JavaScript can use.

Examples include:

- `setTimeout()`
- `setInterval()`
- DOM APIs
- Fetch API
- Browser event handling

When JavaScript encounters an asynchronous browser operation, the runtime environment can handle that operation outside the normal execution of JavaScript code.

### Example

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 2000);

console.log("End");
```

The browser handles the timer while JavaScript continues executing.

When the timer becomes ready, its callback is scheduled for execution.

---

## 6. Callback Queue (Task Queue)

The Callback Queue, also called the Task Queue, holds callbacks that are ready to execute.
For example, when a timer finishes, its callback can be placed in this queue.
The callback does not immediately interrupt the currently running code.
It must wait until the Call Stack is empty.

### Example

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

### Output

```text
A
C
B
```

Even though the timer delay is `0`, the callback does not execute immediately.

It waits until the current synchronous code finishes.

---

## 7. Event Loop

The Event Loop coordinates the execution of asynchronous callbacks.

Its main responsibility is to check whether the Call Stack is empty and whether queued callbacks are ready to execute.

### Basic Working

1. JavaScript executes synchronous code.
2. Asynchronous operations are handled by the runtime environment.
3. Ready callbacks are placed into their respective queues.
4. The Event Loop checks the Call Stack.
5. When the stack is empty, queued callbacks can be moved for execution.

### Important

The Event Loop does not execute JavaScript code itself. It coordinates when queued callbacks can enter the Call Stack.

---

## 8. Microtask Queue

JavaScript has different queues for asynchronous callbacks.

The Microtask Queue has higher priority than the normal Callback Queue.

Common microtasks include:

- Promise `.then()`
- Promise `.catch()`
- Promise `.finally()`
- `queueMicrotask()`

### Example

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

### Output

```text
Start
End
Promise
Timeout
```

### Explanation

1. `"Start"` executes.
2. The timer callback is scheduled.
3. The Promise callback is added to the Microtask Queue.
4. `"End"` executes.
5. Synchronous code finishes.
6. Microtasks execute first.
7. The timer callback executes afterward.

**Important Rule:** After synchronous code finishes, JavaScript processes available microtasks before moving to the next normal task.

---

## 9. setTimeout()

`setTimeout()` schedules a function to execute once after a specified delay.

### Syntax

```javascript
setTimeout(callback, delay);
```

### Example

```javascript
setTimeout(() => {
  console.log("Hello JavaScript");
}, 2000);
```

The callback becomes eligible to execute after approximately two seconds.

### Clearing a Timeout

`setTimeout()` returns a timer ID. We can use that ID with `clearTimeout()` to cancel the scheduled callback.

```javascript
const timer = setTimeout(() => {
  console.log("This will not execute");
}, 3000);

clearTimeout(timer);
```

The callback is cancelled before it executes.

---

## 10. setInterval()

`setInterval()` schedules a function to execute repeatedly after a specified interval.

### Syntax

```javascript
setInterval(callback, delay);
```

### Example

```javascript
let count = 1;

const timer = setInterval(() => {
  console.log(count);

  count++;

  if (count > 3) {
    clearInterval(timer);
  }
}, 1000);
```

### Output

```text
1
2
3
```

### Clearing an Interval

`clearInterval()` stops a repeating interval.

```javascript
const timer = setInterval(() => {
  console.log("Running...");
}, 1000);

clearInterval(timer);
```

### setTimeout() vs setInterval()

| setTimeout()                | setInterval()                |
| --------------------------- | ---------------------------- |
| Executes once               | Executes repeatedly          |
| Uses clearTimeout()         | Uses clearInterval()         |
| Schedules a single callback | Schedules repeated callbacks |

**Important:** Timer delays are not guaranteed exact execution times. A callback may run later if JavaScript is busy.

---

## 11. Callback Functions

A callback is a function passed as an argument to another function.

The receiving function can call that callback when needed.

### Example

```javascript
function greet(name) {
  console.log("Hello " + name);
}

function processUser(callback) {
  callback("Ali");
}

processUser(greet);
```

### Output

```text
Hello Ali
```

Here:

- `greet` is the callback function.
- `processUser` receives it as an argument.
- `callback("Ali")` executes the passed function.

### Passing a Function vs Calling a Function

```javascript
processUser(greet);
```

This passes the function itself.

```javascript
processUser(greet());
```

This calls `greet()` immediately and passes its return value.

These two statements are not the same.

### Synchronous Callback

A callback does not have to be asynchronous.

```javascript
function calculate(callback) {
  console.log("Calculating...");

  callback();
}

calculate(() => {
  console.log("Finished");
});
```

### Output

```text
Calculating...
Finished
```

The callback executes immediately as part of the normal synchronous execution.

---

## 12. Callback Hell

Callback Hell happens when multiple asynchronous operations are nested inside one another.

As the number of operations increases, the code becomes deeply nested and difficult to understand.

### Example

```javascript
getUser(function (user) {
  getOrders(user.id, function (orders) {
    getPayment(orders[0].id, function (payment) {
      getDetails(payment.id, function (details) {
        console.log(details);
      });
    });
  });
});
```

### Problems with Callback Hell

1. Code becomes deeply nested.
2. Reading and understanding becomes difficult.
3. Error handling becomes complicated.
4. Maintaining the code becomes harder.

Promises and Async/Await provide cleaner ways to handle asynchronous operations.

---

# Quick Revision

| Concept         | Meaning                                                           |
| --------------- | ----------------------------------------------------------------- |
| Synchronous     | Executes statements one by one                                    |
| Asynchronous    | Allows operations to finish later without blocking remaining code |
| Call Stack      | Tracks active function calls                                      |
| Web APIs        | Browser-provided facilities                                       |
| Callback Queue  | Holds ready normal task callbacks                                 |
| Event Loop      | Coordinates callback execution                                    |
| Microtask Queue | Holds Promise-related microtasks                                  |
| setTimeout()    | Executes a callback once after a delay                            |
| setInterval()   | Repeats a callback                                                |
| Callback        | Function passed to another function                               |
| Callback Hell   | Deeply nested callbacks                                           |

## Important Points to Remember

- JavaScript normally executes synchronous code first.
- The Call Stack follows LIFO.
- Browser APIs handle operations such as timers.
- The Event Loop coordinates queued callbacks.
- Promise callbacks are microtasks.
- Microtasks have priority over normal task callbacks.
- `setTimeout()` executes once.
- `setInterval()` repeats.
- A callback can be synchronous or asynchronous.
- Callback Hell makes asynchronous code difficult to maintain.

**End of Part 1 — Asynchronous JavaScript Fundamentals**
