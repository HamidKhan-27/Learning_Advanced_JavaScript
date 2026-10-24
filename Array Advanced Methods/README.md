# Advanced JavaScript #5 — Array Advanced Methods

## 1. Array Advanced Methods

JavaScript mein arrays ke saath kaam karne ke liye kuch powerful built-in methods hain.

Is topic ke important methods:

- `map()`
- `filter()`
- `find()`
- `some()`
- `every()`
- `reduce()`

### Easy Memory Trick

| Method     | Kaam                        | Return       |
| ---------- | --------------------------- | ------------ |
| `map()`    | Values ko change/transform  | New Array    |
| `filter()` | Kuch values select          | New Array    |
| `find()`   | Pehli matching value        | Single Value |
| `some()`   | Kya koi ek match karta hai? | `true/false` |
| `every()`  | Kya sab match karte hain?   | `true/false` |
| `reduce()` | Multiple values ko combine  | Single Value |

---

# 2. `map()`

`map()` array ke **har element par kaam karta hai** aur ek **new array** return karta hai.

### Syntax

```js
const newArray = array.map((value) => {
  return newValue;
});
```

### Example

```js
const numbers = [2, 4, 6, 8];

const double = numbers.map((num) => {
  return num * 2;
});

console.log(double);
```

Output:

```text
[4, 8, 12, 16]
```

Original array:

```text
[2, 4, 6, 8]
```

`map()` ne original array ko replace nahi kiya.

### Short Arrow Function

```js
const numbers = [2, 4, 6, 8];

const double = numbers.map((num) => num * 2);

console.log(double);
```

---

# 3. `map()` with Objects

Objects ke array mein `map()` bohot useful hota hai.

```js
const users = [
  { name: "Hamid", age: 18 },
  { name: "Ali", age: 20 },
  { name: "Ahmed", age: 22 },
];

const names = users.map((user) => user.name);

console.log(names);
```

Output:

```text
["Hamid", "Ali", "Ahmed"]
```

Yahan:

```js
user.name;
```

har object ka `name` le raha hai.

---

# 4. `map()` Callback ka Index

`map()` callback mein value ke saath index bhi mil sakta hai.

```js
const fruits = ["Apple", "Banana", "Mango"];

fruits.map((fruit, index) => {
  console.log(index, fruit);
});
```

Output:

```text
0 Apple
1 Banana
2 Mango
```

Basic form:

```js
array.map((value, index) => {
  // code
});
```

---

# 5. `filter()`

`filter()` array mein se sirf woh values select karta hai jo condition ko satisfy karti hain.

Ye bhi **new array** return karta hai.

### Syntax

```js
const newArray = array.filter((value) => {
  return condition;
});
```

### Example

```js
const numbers = [1, 2, 3, 4, 5, 6];

const even = numbers.filter((num) => num % 2 === 0);

console.log(even);
```

Output:

```text
[2, 4, 6]
```

`filter()` mein:

- condition `true` → value include hogi
- condition `false` → value include nahi hogi

---

# 6. `filter()` with Objects

```js
const users = [
  { name: "Hamid", age: 18 },
  { name: "Ali", age: 15 },
  { name: "Ahmed", age: 22 },
];

const adults = users.filter((user) => user.age >= 18);

console.log(adults);
```

Output:

```js
[
  { name: "Hamid", age: 18 },
  { name: "Ahmed", age: 22 },
];
```

---

# 7. `filter()` with Index

`filter()` ko bhi value aur index mil sakta hai.

```js
const fruits = ["Apple", "Banana", "Mango"];

const result = fruits.filter((fruit, index) => {
  return index > 0;
});

console.log(result);
```

Output:

```text
["Banana", "Mango"]
```

---

# 8. `find()`

`find()` condition ko check karta hai aur **pehli matching value** return karta hai.

### Syntax

```js
const result = array.find((value) => {
  return condition;
});
```

### Example

```js
const numbers = [20, 40, 60, 80];

const greater = numbers.find((num) => num > 50);

console.log(greater);
```

Output:

```text
60
```

Important:

`find()` ko `60` mil gaya, isliye uske baad `80` ko return nahi karega.

### `find()` vs `filter()`

```js
const numbers = [20, 40, 60, 80];

numbers.find((num) => num > 50);
```

Result:

```text
60
```

`filter()`:

```js
numbers.filter((num) => num > 50);
```

Result:

```text
[60, 80]
```

**Difference:**

- `find()` → first matching value
- `filter()` → all matching values

---

# 9. `some()`

`some()` check karta hai:

> Kya array mein **kam az kam ek** element condition satisfy karta hai?

Result hamesha:

```js
true;
```

ya

```js
false;
```

### Example

```js
const numbers = [20, 50, 120, 40];

const result = numbers.some((num) => num > 100);

console.log(result);
```

Output:

```text
true
```

Kyun?

Kyuki `120 > 100` hai.

### Agar koi match na kare

```js
const numbers = [20, 30, 40];

const result = numbers.some((num) => num > 100);

console.log(result);
```

Output:

```text
false
```

### Memory

`some()` = **At least one?**

---

# 10. `every()`

`every()` check karta hai:

> Kya array ke **sabhi elements** condition satisfy karte hain?

Result:

```js
true;
```

ya

```js
false;
```

### Example

```js
const numbers = [20, 30, 40, 50];

const result = numbers.every((num) => num > 10);

console.log(result);
```

Output:

```text
true
```

Kyunki sab numbers `10` se greater hain.

### Ek bhi fail ho

```js
const numbers = [20, 30, 5, 50];

const result = numbers.every((num) => num > 10);

console.log(result);
```

Output:

```text
false
```

Kyuki `5 > 10` false hai.

### Memory

`every()` = **All?**

---

# 11. `some()` vs `every()`

```js
const numbers = [20, 30, 40];
```

### `some()`

```js
numbers.some((num) => num > 35);
```

Result:

```text
true
```

Kyuki `40` condition satisfy karta hai.

### `every()`

```js
numbers.every((num) => num > 35);
```

Result:

```text
false
```

Kyuki 20 aur 30 condition satisfy nahi karte.

**Yaad rakho:**

```text
some()  → koi ek bhi?
every() → sabhi?
```

---

# 12. `reduce()`

`reduce()` multiple array values ko **combine karke ek final value** banata hai.

Ye total, sum, maximum, counting aur doosre calculations ke liye useful hai.

### Basic Syntax

```js
const result = array.reduce((accumulator, value) => {
  return accumulator + value;
}, initialValue);
```

---

# 13. `reduce()` — Total

```js
const prices = [100, 200, 300, 400];

const total = prices.reduce((sum, num) => {
  return sum + num;
}, 0);

console.log(total);
```

Output:

```text
1000
```

Yahan:

- `sum` → accumulator
- `num` → current value
- `0` → initial value

Calculation:

```text
0 + 100 = 100
100 + 200 = 300
300 + 300 = 600
600 + 400 = 1000
```

---

# 14. Accumulator

`reduce()` mein accumulator previous calculation ka result hold karta hai.

```js
const numbers = [10, 20, 30];

const total = numbers.reduce((sum, num) => {
  return sum + num;
}, 0);
```

Step-by-step:

```text
sum = 0,  num = 10 → 10
sum = 10, num = 20 → 30
sum = 30, num = 30 → 60
```

Final:

```text
60
```

---

# 15. Initial Value

Usually `reduce()` ke saath initial value dena clear aur safe approach hai.

```js
numbers.reduce((sum, num) => sum + num, 0);
```

Yahan:

```js
0;
```

initial accumulator hai.

Agar simple numeric array mein initial value na bhi dein:

```js
const total = prices.reduce((sum, num) => sum + num);
```

to JavaScript first value ko initial accumulator bana sakta hai.

Lekin `0` explicitly dena code ko zyada clear banata hai.

---

# 16. `reduce()` for Maximum Value

`reduce()` sirf total ke liye nahi hota.

```js
const numbers = [20, 70, 40, 90, 50];

const max = numbers.reduce((highest, num) => {
  return num > highest ? num : highest;
}, numbers[0]);

console.log(max);
```

Output:

```text
90
```

---

# 17. Method Chaining

Array methods ko ek ke baad doosre method ke saath use kar sakte hain.

Isko **method chaining** kehte hain.

Example:

```js
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers.filter((num) => num % 2 === 0).map((num) => num * 2);

console.log(result);
```

Pehle:

```js
filter();
```

even numbers:

```text
[2, 4, 6]
```

Phir:

```js
map();
```

double:

```text
[4, 8, 12]
```

Final:

```text
[4, 8, 12]
```

---

# 18. Array of Objects + Methods

Real projects mein arrays mein objects bohot common hain.

```js
const products = [
  { name: "Laptop", price: 80000 },
  { name: "Mouse", price: 2000 },
  { name: "Keyboard", price: 5000 },
];
```

### Product Names

```js
const names = products.map((product) => product.name);

console.log(names);
```

### Expensive Products

```js
const expensive = products.filter((product) => product.price > 5000);

console.log(expensive);
```

### Specific Product

```js
const product = products.find((product) => product.name === "Mouse");

console.log(product);
```

---

# 19. Callback Function

In methods mein jo function hum pass karte hain usay **callback function** kehte hain.

Example:

```js
numbers.map((num) => num * 2);
```

Yahan:

```js
(num) => num * 2;
```

callback function hai.

Isi tarah:

```js
numbers.filter((num) => num > 10);
```

mein bhi arrow function callback hai.

Detailed callback topic baad mein separately study hoga.

---

# 20. Common Mistake — `map()` mein `return`

Agar arrow function mein `{}` use kar rahe ho to `return` yaad rakho.

### Correct

```js
const result = numbers.map((num) => {
  return num * 2;
});
```

### Incorrect

```js
const result = numbers.map((num) => {
  num * 2;
});
```

Is case mein value return nahi hogi.

### Short syntax

Agar `{}` nahi hain:

```js
const result = numbers.map((num) => num * 2);
```

to implicit return hota hai.

---

# 21. Common Mistake — `filter()`

`filter()` ko condition return karni hoti hai.

Correct:

```js
const even = numbers.filter((num) => num % 2 === 0);
```

Yahan condition:

```js
num % 2 === 0;
```

`true` ya `false` return karti hai.

---

# 22. Original Array

Normally:

```js
map();
```

aur

```js
filter();
```

original array ko change nahi karte.

Example:

```js
const numbers = [1, 2, 3];

const doubled = numbers.map((num) => num * 2);

console.log(numbers);
console.log(doubled);
```

Output:

```text
[1, 2, 3]
[2, 4, 6]
```

Original:

```text
[1, 2, 3]
```

same hai.

---

# 23. Quick Comparison

| Method     | Main Purpose           | Result        |
| ---------- | ---------------------- | ------------- |
| `map()`    | Har value transform    | Array         |
| `filter()` | Matching values select | Array         |
| `find()`   | First matching value   | Value         |
| `some()`   | At least one match?    | Boolean       |
| `every()`  | All match?             | Boolean       |
| `reduce()` | Values combine         | Single result |

### One-line Memory

```text
map    = Change
filter = Select
find   = First
some   = At least one
every  = All
reduce = Combine
```

---

# 24. Real-World Example

```js
const users = [
  { name: "Hamid", age: 18 },
  { name: "Ali", age: 16 },
  { name: "Ahmed", age: 22 },
];
```

### Names

```js
const names = users.map((user) => user.name);
```

### Adults

```js
const adults = users.filter((user) => user.age >= 18);
```

### First Adult

```js
const firstAdult = users.find((user) => user.age >= 18);
```

### Is Anyone Adult?

```js
const hasAdult = users.some((user) => user.age >= 18);
```

### Are All Adults?

```js
const allAdults = users.every((user) => user.age >= 18);
```

Ye methods real applications mein data display, filtering, searching aur calculations ke liye bohot use hote hain.

---

# 25. Final Revision

### `map()`

```js
const result = numbers.map((num) => num * 2);
```

**Har element ko transform karta hai.**

### `filter()`

```js
const result = numbers.filter((num) => num > 10);
```

**Condition ke according elements select karta hai.**

### `find()`

```js
const result = numbers.find((num) => num > 10);
```

**Pehli matching value deta hai.**

### `some()`

```js
const result = numbers.some((num) => num > 10);
```

**Kya kam az kam ek match karta hai?**

### `every()`

```js
const result = numbers.every((num) => num > 10);
```

**Kya sab match karte hain?**

### `reduce()`

```js
const total = numbers.reduce((sum, num) => sum + num, 0);
```

**Multiple values ko ek result mein combine karta hai.**

---

# 26. Important Points

- `map()` new array return karta hai.
- `filter()` new array return karta hai.
- `find()` sirf first matching value return karta hai.
- `some()` boolean return karta hai.
- `every()` boolean return karta hai.
- `reduce()` usually single final result deta hai.
- `map()` aur `filter()` ko chain kiya ja sakta hai.
- Array of objects ke saath ye methods bohot useful hain.
- `map()` ke block `{}` mein `return` zaroori hota hai.
- `filter()` mein condition ka `true/false` result important hai.
- `reduce()` mein accumulator previous result ko hold karta hai.
- `forEach()` aur callback functions ka detailed use later topics mein aur study hoga.

## Master Formula

```js
array.map(...)
array.filter(...)
array.find(...)
array.some(...)
array.every(...)
array.reduce(...)
```

In 6 methods ko samajhna JavaScript mein arrays ke saath modern aur practical programming ke liye bohot important hai.
