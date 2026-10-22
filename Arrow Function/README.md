# Advanced JavaScript #3 — Arrow Functions

## 1. Arrow Functions kya hain?

Arrow Function JavaScript mein function likhne ka modern aur shorter syntax hai.

Normal function:

```js
function greet() {
    console.log("Hello");
}
```

Arrow function:

```js
const greet = () => {
    console.log("Hello");
};
```

Arrow Function ki pehchan `=>` symbol hai.

---

## 2. Basic Syntax

```js
const functionName = () => {
    // code
};
```

Example:

```js
const greet = () => {
    console.log("Hello Hamid");
};

greet();
```

Output:

```text
Hello Hamid
```

### Syntax breakdown

```js
const greet
```

Function ko variable mein store karta hai.

```js
()
```

Parameters ke liye.

```js
=>
```

Arrow Function ka symbol.

```js
{}
```

Function body.

---

## 3. Parameters

Arrow Function mein parameters `()` ke andar likhte hain.

```js
const greet = (name) => {
    console.log(`Hello ${name}`);
};

greet("Hamid");
```

Output:

```text
Hello Hamid
```

Multiple parameters:

```js
const add = (a, b) => {
    console.log(a + b);
};

add(10, 20);
```

Output:

```text
30
```

---

## 4. Single Parameter

Sirf **one parameter** ho to parentheses optional hain.

With parentheses:

```js
const square = (num) => {
    return num * num;
};
```

Without parentheses:

```js
const square = num => {
    return num * num;
};
```

Dono valid hain.

```js
console.log(square(5));
```

Output:

```text
25
```

---

## 5. Zero Parameters

Agar koi parameter nahi hai to empty `()` zaroori hain.

```js
const greet = () => {
    console.log("Hello");
};

greet();
```

Wrong:

```js
const greet = => {
    console.log("Hello");
};
```

Correct:

```js
const greet = () => {
    console.log("Hello");
};
```

---

## 6. Return Statement

Arrow Function normal function ki tarah `return` use kar sakta hai.

```js
const add = (a, b) => {
    return a + b;
};

let result = add(10, 20);

console.log(result);
```

Output:

```text
30
```

`return` function ka result wapas bhejta hai.

---

## 7. Implicit Return

Arrow Function mein agar sirf **ek expression return** karna ho to `{}` aur `return` hata sakte hain.

Explicit return:

```js
const add = (a, b) => {
    return a + b;
};
```

Implicit return:

```js
const add = (a, b) => a + b;
```

Dono ka result same hai:

```js
console.log(add(10, 20));
```

Output:

```text
30
```

### Simple example

```js
const double = num => num * 2;

console.log(double(5));
```

Output:

```text
10
```

---

## 8. Multiple Statements

Agar function mein multiple statements hain to `{}` use karte hain aur result ke liye `return` likhte hain.

```js
const calculate = (a, b) => {
    let sum = a + b;
    let result = sum * 2;

    return result;
};

console.log(calculate(10, 20));
```

Output:

```text
60
```

---

## 9. Arrow Function + Template Literals

Arrow Functions ko Template Literals ke saath use kar sakte hain.

```js
const greet = name => `Hello ${name}`;

console.log(greet("Hamid"));
```

Output:

```text
Hello Hamid
```

Yahan Template Literal implicit return ho raha hai.

---

## 10. Arrow Function + Condition

Ternary operator ko bhi return kar sakte hain:

```js
const checkAge = age =>
    age >= 18 ? "Adult" : "Minor";

console.log(checkAge(20));
```

Output:

```text
Adult
```

---

## 11. Arrow Function + Callback

Arrow Function ko kisi doosre function ko argument ke taur par bhi diya ja sakta hai.

```js
let numbers = [1, 2, 3];

numbers.forEach((num) => {
    console.log(num);
});
```

Yahan:

```js
(num) => {
    console.log(num);
}
```

Arrow Function hai.

**Note:** Callback Functions ko later separate topic mein detail se study karenge.

---

## 12. Object Return

Arrow Function se directly object return karte waqt object ko `()` mein wrap karna hota hai.

Correct:

```js
const getUser = () => ({
    name: "Hamid",
    city: "Karachi"
});

console.log(getUser());
```

Output:

```text
{ name: "Hamid", city: "Karachi" }
```

Important syntax:

```js
() => ({
    key: "value"
})
```

Direct `{}` use karne se JavaScript usse function body samajhta hai:

```js
const getUser = () => {
    name: "Hamid";
};
```

Isliye object ko parentheses mein wrap karte hain.

---

## 13. `this` ka Basic Difference

Arrow Functions ka traditional functions se ek important difference `this` hai.

Traditional function:

```js
const user = {
    name: "Hamid",

    greet: function () {
        console.log(this.name);
    }
};

user.greet();
```

Output:

```text
Hamid
```

Arrow Function ka **apna `this` nahi hota**. Ye surrounding scope ka `this` use karta hai.

Basic rule:

> Arrow Function apna separate `this` create nahi karta.

`this` ko OOP aur advanced JavaScript mein detail se study kiya jayega.

---

## 14. Common Mistakes

### Mistake 1 — `=>` missing

Wrong:

```js
const greet = () {
    console.log("Hello");
};
```

Correct:

```js
const greet = () => {
    console.log("Hello");
};
```

### Mistake 2 — Multiple parameters

Wrong:

```js
const add = a, b => a + b;
```

Correct:

```js
const add = (a, b) => a + b;
```

### Mistake 3 — Implicit return samajhna

Wrong:

```js
const add = (a, b) => {
    a + b;
};
```

Ismein result return nahi hota.

Correct:

```js
const add = (a, b) => a + b;
```

Ya:

```js
const add = (a, b) => {
    return a + b;
};
```

### Mistake 4 — Function call bhoolna

Function banana:

```js
const greet = () => {
    console.log("Hello");
};
```

Execute karna:

```js
greet();
```

---

## 15. Arrow Function kab useful hai?

Arrow Functions especially useful hain:

* Short functions ke liye
* Simple calculations ke liye
* Callbacks ke liye
* Implicit return ke liye
* Modern JavaScript code mein

Example:

```js
const add = (a, b) => a + b;
```

Lekin `this` ke different behavior ki wajah se har situation mein Arrow Function use karna zaroori nahi.

---

# Quick Revision

### No parameters

```js
const greet = () => {
    console.log("Hello");
};
```

### One parameter

```js
const square = num => num * num;
```

### Multiple parameters

```js
const add = (a, b) => a + b;
```

### Explicit return

```js
const add = (a, b) => {
    return a + b;
};
```

### Implicit return

```js
const add = (a, b) => a + b;
```

### Object return

```js
const getUser = () => ({
    name: "Hamid"
});
```

### Template Literal

```js
const greet = name => `Hello ${name}`;
```

### Condition

```js
const checkAge = age =>
    age >= 18 ? "Adult" : "Minor";
```

---

# Main Syntax

General:

```js
const functionName = (parameters) => {
    // code
};
```

Short function:
```js
const functionName = (parameters) => expression;
```
Single parameter:
```js
const functionName = parameter => expression;
```
Object return:
```js
const functionName = () => ({
    key: "value"
});
```
---

# Final Revision

Arrow Function ka basic idea:

```text
Normal Function
      ↓
Short ES6 Syntax
      ↓
Arrow Function
```

Sabse important patterns:

```js
() => {}

(num) => num * 2

(a, b) => a + b

(a, b) => {
    return a + b;
}

() => ({
    name: "Hamid"
})
```

### Yaad rakhne wali 5 baatein

1. `=>` Arrow Function ki identity hai.
2. Zero parameters ke liye `()` required hain.
3. One parameter mein `()` optional hain.
4. Multiple parameters mein `()` required hain.
5. Single expression mein implicit return use kar sakte hain.

**#3 Arrow Functions — Complete.**
