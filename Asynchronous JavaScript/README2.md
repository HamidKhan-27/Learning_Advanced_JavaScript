# Topic #15 — Asynchronous JavaScript (Part 2)
## Promises and Async/Await

### Topics Covered
1. What is a Promise?
2. Promise States
3. Creating a Promise
4. resolve() and reject()
5. then()
6. catch()
7. finally()
8. Promise Chaining
9. Returning Values from then()
10. Returning Promises
11. Error Handling
12. Async Functions
13. Await Keyword
14. Try...Catch
15. Promise.all()
16. Practical Example

---

## 1. What is a Promise?

A Promise is a JavaScript object that represents the eventual result of an asynchronous operation.

That result may be:
- A successful value.
- An error or failure.

For example, when an application requests data from a server, the response may take some time.

A Promise allows us to handle that future result.

### Example

```javascript
const promise = new Promise((resolve, reject) => {
    resolve("Data received");
});

console.log(promise);
```

The Promise represents an operation that has been completed successfully.

---

## 2. Promise States

A Promise has three possible states.

### 1. Pending

The operation has not completed yet.

### 2. Fulfilled

The operation completed successfully.

### 3. Rejected

The operation failed.

### Promise State Flow

```text
          Pending
          /     \
         /       \
   Fulfilled   Rejected
```

A Promise starts as pending and eventually becomes fulfilled or rejected.

Once settled, its state does not change again.

**Settled** means the Promise is either fulfilled or rejected.

---

## 3. Creating a Promise

We create a Promise using the `Promise` constructor.

### Syntax

```javascript
const promise = new Promise((resolve, reject) => {
    // Asynchronous operation
});
```

The constructor receives an executor function.

This executor function runs immediately and synchronously when the Promise is created.

### Example

```javascript
console.log("Start");

const promise = new Promise((resolve, reject) => {
    console.log("Promise executor");

    resolve("Success");
});

console.log("End");
```

### Output

```text
Start
Promise executor
End
```

### Explanation

1. `"Start"` is printed.
2. A Promise is created.
3. Its executor function runs immediately.
4. `"Promise executor"` is printed.
5. The Promise is fulfilled.
6. `"End"` is printed.

Remember: The Promise executor itself does not automatically run asynchronously.

---

## 4. resolve() and reject()

The Promise executor receives two functions:

- `resolve()` fulfills the Promise.
- `reject()` rejects the Promise.

### Using resolve()

```javascript
const promise = new Promise((resolve, reject) => {
    resolve("Operation successful");
});
```

The Promise becomes fulfilled.

### Using reject()

```javascript
const promise = new Promise((resolve, reject) => {
    reject("Something went wrong");
});
```

The Promise becomes rejected.

Only the first effective settlement determines the Promise's state.

---

## 5. then()

The `.then()` method handles a fulfilled Promise.

It receives the fulfilled value as an argument.

### Syntax

```javascript
promise.then((result) => {
    // Handle successful result
});
```

### Example

```javascript
const promise = new Promise((resolve) => {
    resolve("Welcome");
});

promise.then((result) => {
    console.log(result);
});
```

### Output

```text
Welcome
```

The value passed to `resolve()` is received by the `.then()` callback.

### Important

`.then()` returns a new Promise, which makes Promise chaining possible.

---

## 6. catch()

The `.catch()` method handles a rejected Promise.

### Syntax

```javascript
promise.catch((error) => {
    // Handle error
});
```

### Example

```javascript
const promise = new Promise((resolve, reject) => {
    reject("Request failed");
});

promise.catch((error) => {
    console.log(error);
});
```

### Output

```text
Request failed
```

`.catch()` is commonly used to handle errors in asynchronous operations.

---

## 7. finally()

The `.finally()` method executes after a Promise settles, whether it is fulfilled or rejected.

It does not receive the fulfillment value or rejection reason as its normal callback argument.

### Example

```javascript
const promise = new Promise((resolve) => {
    resolve("Success");
});

promise
    .then((result) => {
        console.log(result);
    })
    .finally(() => {
        console.log("Operation completed");
    });
```

### Output

```text
Success
Operation completed
```

### Common Use

`finally()` is useful for cleanup tasks, such as hiding a loading indicator after a request finishes.

---

## 8. Promise Chaining

Promise chaining allows us to perform multiple operations one after another.

We can attach multiple `.then()` methods to a Promise.

### Example

```javascript
Promise.resolve(5)
    .then((number) => {
        console.log(number);

        return number * 2;
    })
    .then((result) => {
        console.log(result);

        return result + 5;
    })
    .then((finalResult) => {
        console.log(finalResult);
    });
```

### Output

```text
5
10
15
```

### Explanation

1. The first `.then()` receives `5`.
2. It returns `10`.
3. The second `.then()` receives `10`.
4. It returns `15`.
5. The third `.then()` receives `15`.

Each `.then()` receives the value returned by the previous `.then()`.

---

## 9. Returning Values from then()

Returning a value from `.then()` is important because it determines what the next `.then()` receives.

### Example

```javascript
Promise.resolve(10)
    .then((value) => {
        return value + 5;
    })
    .then((result) => {
        console.log(result);
    });
```

### Output

```text
15
```

The first `.then()` returns `15`, so the next `.then()` receives `15`.

### What Happens if We Do Not Return?

```javascript
Promise.resolve(10)
    .then((value) => {
        value + 5;
    })
    .then((result) => {
        console.log(result);
    });
```

### Output

```text
undefined
```

The first `.then()` does not return anything, so its returned Promise fulfills with `undefined`.

**Important:** If you need to pass a calculated value to the next `.then()`, return it.

---

## 10. Returning Another Promise

A `.then()` callback can return another Promise.

The next `.then()` waits for that returned Promise to settle.

### Example

```javascript
Promise.resolve(5)
    .then((value) => {
        return new Promise((resolve) => {
            setTimeout(() => {
                resolve(value * 2);
            }, 1000);
        });
    })
    .then((result) => {
        console.log(result);
    });
```

### Output

```text
10
```

The second `.then()` waits for the Promise returned by the first `.then()`.

It receives the fulfilled value `10`, not the Promise object itself.

---

## 11. Error Handling in Promise Chains

We can handle errors using `.catch()`.

### Example

```javascript
Promise.reject("Something went wrong")
    .then((result) => {
        console.log(result);
    })
    .catch((error) => {
        console.log("Error:", error);
    });
```

### Output

```text
Error: Something went wrong
```

If an error occurs in a `.then()` callback, the returned Promise becomes rejected and a later `.catch()` can handle it.

### Example

```javascript
Promise.resolve(10)
    .then((value) => {
        throw new Error("Invalid operation");
    })
    .catch((error) => {
        console.log(error.message);
    });
```

### Output

```text
Invalid operation
```

---

## 12. Async Functions

The `async` keyword is used to declare an asynchronous function.

An async function always returns a Promise.

### Example

```javascript
async function greet() {
    return "Hello";
}

const result = greet();

console.log(result);
```

The returned value is a Promise fulfilled with `"Hello"`.

### Handling the Result

```javascript
async function greet() {
    return "Hello";
}

greet().then((message) => {
    console.log(message);
});
```

### Output

```text
Hello
```

Even when an async function returns a normal value, JavaScript wraps that value in a fulfilled Promise.

---

## 13. Await Keyword

The `await` keyword is used inside an async function to wait for a Promise to settle.

If the Promise fulfills, `await` gives us its fulfilled value.

### Example

```javascript
function getData() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("Data received");
        }, 2000);
    });
}

async function showData() {
    const result = await getData();

    console.log(result);
}

showData();
```

### Output

```text
Data received
```

### Explanation

1. `showData()` starts.
2. `getData()` returns a Promise.
3. `await` pauses the remaining execution of `showData()`.
4. JavaScript can continue executing other code.
5. When the Promise fulfills, execution of `showData()` resumes.
6. The fulfilled value is stored in `result`.

**Important:** `await` does not block the entire JavaScript thread. It pauses the execution of the current async function.

---

## 14. Try...Catch with Async/Await

When an awaited Promise is rejected, `await` throws the rejection reason.

We can handle it using `try...catch`.

### Example

```javascript
function getData() {
    return new Promise((resolve, reject) => {
        reject("Unable to fetch data");
    });
}

async function showData() {
    try {
        const result = await getData();

        console.log(result);
    } catch (error) {
        console.log("Error:", error);
    }
}

showData();
```

### Output

```text
Error: Unable to fetch data
```

### Explanation

- `try` contains code that may fail.
- `catch` handles the error.
- If the Promise is rejected, execution moves to the `catch` block.

This makes error handling with async/await easier to read.

---

## 15. Promise.all()

`Promise.all()` accepts an iterable of Promises and returns a new Promise.

It fulfills when all supplied Promises fulfill.

The result is an array containing their fulfilled values in the same order as the input Promises.

If any supplied Promise rejects, the Promise returned by `Promise.all()` rejects.

### Example

```javascript
const promise1 = Promise.resolve("Apple");
const promise2 = Promise.resolve("Banana");
const promise3 = Promise.resolve("Mango");

Promise.all([promise1, promise2, promise3])
    .then((results) => {
        console.log(results);
    });
```

### Output

```text
["Apple", "Banana", "Mango"]
```

### When to Use Promise.all()

Use it when multiple independent asynchronous operations need to be completed together.

### Example with Async/Await

```javascript
async function getAllData() {
    const results = await Promise.all([
        Promise.resolve("Users"),
        Promise.resolve("Products"),
        Promise.resolve("Orders")
    ]);

    console.log(results);
}

getAllData();
```

### Output

```text
["Users", "Products", "Orders"]
```

**Important:** `Promise.all()` is useful when operations do not depend on one another. It does not make dependent operations independent.

---

## 16. Practical Example — Fetching User and Orders

Let's understand how Promise chaining works when one operation depends on another.

Suppose we first fetch a user, then use that user's ID to fetch their orders.

### Example

```javascript
function getUser() {
    return Promise.resolve({
        id: 1,
        name: "Ali"
    });
}

function getOrders(userId) {
    if (userId === 1) {
        return Promise.resolve(["Laptop", "Mobile"]);
    }

    return Promise.resolve([]);
}

getUser()
    .then((user) => {
        console.log(user);

        return getOrders(user.id);
    })
    .then((orders) => {
        console.log(orders);
    })
    .catch((error) => {
        console.log(error);
    });
```

### Output

```text
{ id: 1, name: "Ali" }
["Laptop", "Mobile"]
```

### Explanation

1. `getUser()` returns a Promise.
2. The first `.then()` receives the complete user object.
3. `user.id` reads the object's `id` property.
4. `getOrders(user.id)` calls the function with `1`.
5. The `userId` parameter inside `getOrders()` receives `1`.
6. `getOrders()` returns a Promise containing the orders.
7. The next `.then()` receives the fulfilled orders array.

Notice that `user.id` is passed because `getOrders()` actually uses the received ID.

---

## 17. Promise Chaining vs Async/Await

Both approaches can handle asynchronous operations.

### Promise Chaining

```javascript
getUser()
    .then((user) => {
        return getOrders(user.id);
    })
    .then((orders) => {
        console.log(orders);
    })
    .catch((error) => {
        console.log(error);
    });
```

### Async/Await

```javascript
async function showOrders() {
    try {
        const user = await getUser();

        const orders = await getOrders(user.id);

        console.log(orders);
    } catch (error) {
        console.log(error);
    }
}

showOrders();
```

### Comparison

| Promise Chaining | Async/Await |
|---|---|
| Uses `.then()` | Uses `await` |
| Uses `.catch()` for errors | Commonly uses `try...catch` |
| Can become harder to read with complex chains | Often easier to read |
| Returns Promises between chain steps | Async function always returns a Promise |

---

# Quick Revision

| Concept | Meaning |
|---|---|
| Promise | Represents a future asynchronous result |
| Pending | Operation is not completed |
| Fulfilled | Operation completed successfully |
| Rejected | Operation failed |
| resolve() | Fulfills a Promise |
| reject() | Rejects a Promise |
| then() | Handles fulfillment |
| catch() | Handles rejection |
| finally() | Runs after settlement |
| Promise Chaining | Connects multiple Promise operations |
| async | Declares a function that returns a Promise |
| await | Waits for a Promise's result inside an async function |
| try...catch | Handles errors |
| Promise.all() | Waits for all supplied Promises to fulfill |

## Important Points to Remember

- A Promise has three states: pending, fulfilled and rejected.
- The Promise executor runs synchronously.
- `.then()` returns a new Promise.
- Returning a value passes it to the next `.then()`.
- Not returning a value results in `undefined` for the next `.then()`.
- Returning a Promise makes the chain wait for its result.
- `.catch()` handles errors in a Promise chain.
- `.finally()` runs after settlement.
- An async function always returns a Promise.
- `await` pauses the current async function, not the entire JavaScript thread.
- `try...catch` handles rejected awaited Promises.
- `Promise.all()` is useful for independent asynchronous operations.

**End of Part 2 — Promises and Async/Await**

---

# Topic #15 Completed

You have covered the fundamentals of asynchronous JavaScript, the Event Loop, callbacks, Promises, Promise chaining, async/await and error handling.