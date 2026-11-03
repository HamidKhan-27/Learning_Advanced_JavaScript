# JavaScript Advanced Topics

## Topic #10 — First-Class Functions & Higher-Order Functions

### Topics Covered

- First-Class Functions
- Function references vs function calls
- Assigning functions to variables
- Higher-Order Functions (HOF)
- Passing functions as arguments
- Returning functions
- Common mistakes and test corrections

---

## 1. First-Class Functions

JavaScript mein functions ko values ki tarah treat kiya ja sakta hai. Unhein variables mein store, arguments ke taur par pass aur doosre functions se return kiya ja sakta hai.

```javascript
const greet = function () {
  console.log("Hello!");
};

greet(); // Hello!
```

Yahan function ko `greet` variable mein store kiya gaya hai.

### Function Reference vs Function Call

```javascript
function sayHi() {
  console.log("Hi");
}

const x = sayHi; // Function reference copy kiya
x(); // Function execute hua
sayHi(); // Wahi function execute hua
```

| Syntax            | Meaning                                 |
| ----------------- | --------------------------------------- |
| `sayHi`           | Function reference                      |
| `sayHi()`         | Function execute karna                  |
| `const x = sayHi` | Reference ko variable mein assign karna |

**Important:** `sayHi` aur `sayHi()` ek jaise nahi hain.

---

## 2. Higher-Order Functions (HOF)

Aisa function jo doosre function ko argument ke taur par receive kare ya function return kare, Higher-Order Function kehlata hai.

```javascript
function add(a, b) {
  return a + b;
}

function calculate(operation, x, y) {
  return operation(x, y);
}

console.log(calculate(add, 5, 3)); // 8
```

Yahan:

- `add` — pass kiya gaya function.
- `calculate` — Higher-Order Function.
- `operation(x, y)` — received function ko execute karta hai.

Isi `calculate` function ko doosra operation de sakte hain:

```javascript
function multiply(a, b) {
  return a * b;
}

console.log(calculate(multiply, 5, 3)); // 15
```

**Faida:** Function ko argument ke taur par de kar behavior change kar sakte hain, bina `calculate` ko dobara likhe.

---

## 3. Function Return Karna

Higher-Order Function doosra function return bhi kar sakta hai.

```javascript
function outer() {
  return function () {
    return 10;
  };
}

const result = outer();

console.log(result()); // 10
```

- `outer()` returned function deta hai.
- `result` us returned function ka reference rakhta hai.
- `result()` us function ko execute karta hai.

### Important Difference

```javascript
return innerFunction;
```

Function reference return karta hai.

```javascript
return innerFunction();
```

Function execute karke uska return value deta hai.

---

## 4. First-Class vs Higher-Order Functions

| First-Class Functions                                           | Higher-Order Functions                                  |
| --------------------------------------------------------------- | ------------------------------------------------------- |
| Functions ko values ki tarah use karne ki JavaScript capability | Aisa function jo doosra function receive ya return kare |
| Function variable mein store ho sakta hai                       | Function argument receive kar sakta hai                 |
| Function return bhi ho sakta hai                                | Function ko return bhi kar sakta hai                    |

Yaad rakho: First-Class Functions ek language capability hai; Higher-Order Function ek aisa function hai jo is capability ko use karta hai.

---

## 5. Common Mistakes

### Mistake 1: Function ko jaldi execute kar dena

```javascript
function greet() {
  console.log("Hello");
}

function run(callback) {
  callback();
}

run(greet); // Correct
// run(greet()); // Wrong for this example
```

`greet()` foran execute hota hai. Is example mein woh `undefined` return karta hai, isliye `run` ke andar `callback()` call karne par `TypeError` aayega.

### Mistake 2: Returned function ko call na karna

```javascript
function outer() {
  return function () {
    console.log("A");
  };
}

outer(); // Inner function return hota hai, "A" print nahi hota
```

Returned function ko execute karne ke liye:

```javascript
outer()(); // A
```

### Mistake 3: Return value samajhne mein ghalti

```javascript
function first() {
  return 10;
}

function second(fn) {
  return function () {
    return fn() + 5;
  };
}

const result = second(first);
console.log(result()); // 15
```

`second(first)` ek function return karta hai. `result()` us function ko execute karta hai, jo `first()` se `10` lekar `5` add karta hai.

---

## Quick Revision

1. JavaScript functions ko values ki tarah treat kiya ja sakta hai.
2. `greet` function reference hai; `greet()` function call hai.
3. Higher-Order Function doosra function receive ya return karta hai.
4. Function ko argument ke taur par pass karna common pattern hai.
5. Function return karne par zaroori nahi ke returned function foran execute ho.
6. `outer()()` ka pehla call outer function ko aur doosra returned function ko execute karta hai.

### Test Results

- Basic Test: **5/5**
- Hard Test: **8/8**
- Total: **13/13**

**Topic #10 complete — study file ready.**

**Next:** Topic #11 — Callback Functions. Iska test complete hone ke baad hi iski revision file banayenge.
