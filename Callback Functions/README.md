# JavaScript Advanced Study Notes

## Topic #11 — Callback Functions

### 1. Callback Function Kya Hai?

Callback woh function hai jo kisi doosre function ko argument ke taur par pass kiya jata hai, taake doosra function usay execute kar sake.

```js
function greet() {
  console.log("Hello!");
}

function process(callback) {
  callback();
}

process(greet);
```

**Output:**

```text
Hello!
```

- `greet` callback function hai.
- `process` higher-order function hai.
- `callback` parameter mein `greet` ka reference receive karta hai.

### 2. Function Reference vs Function Call

```js
function greet() {
  console.log("Hello");
}

greet; // Function reference
greet(); // Function execute hota hai
```

Callback pass karte waqt aam tor par function reference dete hain:

```js
process(greet); // Correct
process(greet()); // Function foran execute hota hai
```

Agar `greet()` koi value return nahi karta, to woh `undefined` return karega. Agar receiver us `undefined` ko function samajh kar call kare, to TypeError aayega.

### 3. Callback with Parameters

```js
function showName(name) {
  console.log(name);
}

function process(callback) {
  callback("Hamid");
}

process(showName);
```

**Output:** `Hamid`

`callback("Hamid")` mein `"Hamid"` callback ke parameter `name` ko milta hai.

### 4. Callback Se Value Return Karna

```js
function add(a, b) {
  return a + b;
}

function calculate(callback) {
  return callback(5, 3);
}

console.log(calculate(add)); // 8
```

Agar outer function callback ka result return nahi karta:

```js
function calculate(callback) {
  callback(5, 3);
}

console.log(calculate(add)); // undefined
```

**Important:** Inner function ka `return` outer function ko automatically value return nahi karwata. Outer function ko bhi result return karna hota hai.

### 5. Anonymous aur Arrow Callbacks

Anonymous function:

```js
setTimeout(function () {
  console.log("Hello");
}, 1000);
```

Arrow function:

```js
setTimeout(() => {
  console.log("Hello");
}, 1000);
```

Dono callback ke examples hain.

### 6. Synchronous Callback

Synchronous callback current execution ke dauran foran execute hota hai.

```js
console.log("Start");

function run(callback) {
  callback();
}

run(function () {
  console.log("Callback");
});

console.log("End");
```

**Output:**

```text
Start
Callback
End
```

### 7. Asynchronous Callback

```js
console.log("Start");

setTimeout(() => {
  console.log("Callback");
}, 0);

console.log("End");
```

**Output:**

```text
Start
End
Callback
```

`setTimeout` ka callback current synchronous code complete hone ke baad execute hota hai, chahe delay `0` milliseconds ho.

### 8. Event Listener Callback

```js
button.addEventListener("click", greet);
```

- `greet` ka reference pass hota hai.
- Function click event par execute hota hai.
- `greet()` likhne se function foran call ho sakta hai; yeh aam event-listener pattern nahi hai.

### 9. Array Methods Mein Callbacks

```js
const numbers = [1, 2, 3];

const result = numbers.map(function (num) {
  return num * 2;
});

console.log(result); // [2, 4, 6]
```

`map()` har element ke liye callback execute karta hai aur callback se milne wali values ki nayi array return karta hai.

### 10. Callback vs Higher-Order Function

- **Callback:** Function jo doosre function ko pass kiya jata hai.
- **Higher-order function (HOF):** Function jo kisi function ko argument ke taur par receive kare ya function return kare.

Ek hi example mein dono ho sakte hain:

```js
function greet() {
  console.log("Hi");
}

function run(callback) {
  callback();
}

run(greet);
```

### 11. Common Mistakes

1. `greet` ki jagah `greet()` pass kar dena.
2. Non-function value ko function ki tarah call karna.
3. Callback ka result return karna bhool jana.
4. Asynchronous code mein output ka order galat samajhna.
5. `map()` ke callback mein `return` bhool kar `undefined` values hasil karna.

### Quick Revision

- Function ko argument ke taur par pass karna callback pattern hai.
- `callback()` us function ko execute karta hai.
- `return callback()` callback ka result outer function se return kar sakta hai.
- Synchronous callback aam tor par current execution mein chalta hai.
- Asynchronous callbacks baad mein execute ho sakte hain.
- Event listeners aur array methods callbacks ke common examples hain.
