# Advanced JavaScript #1 — Var vs Let vs Const

## 1. Introduction

JavaScript mein variables declare karne ke liye 3 keywords hain:

```js
var
let
const
```

Teeno ka purpose variable banana hai, lekin **scope, reassignment aur redeclaration** mein difference hai.

| Feature                 | var | let | const |
| ----------------------- | --- | --- | ----- |
| Reassign                | ✅  | ✅  | ❌    |
| Redeclare               | ✅  | ❌  | ❌    |
| Block Scope             | ❌  | ✅  | ✅    |
| Function Scope          | ✅  | ❌  | ❌    |
| Initialization required | ❌  | ❌  | ✅    |

---

# 2. `var`

`var` JavaScript mein purana variable keyword hai.

```js
var name = "Hamid";

console.log(name);
```

### Reassignment

Value change kar sakte hain:

```js
var age = 20;

age = 21;

console.log(age);
```

### Redeclaration

Same scope mein dobara declare bhi kar sakte hain:

```js
var city = "Karachi";

var city = "Lahore";

console.log(city);
```

Output:

```text
Lahore
```

> **Important:** `var` redeclaration allow karta hai.

---

# 3. `let`

`let` ES6 mein introduce hua aur modern JavaScript mein commonly use hota hai.

```js
let age = 20;
```

### Reassignment

`let` ki value change kar sakte hain:

```js
let age = 20;

age = 25;

console.log(age);
```

### Redeclaration

Same scope mein `let` ko dobara declare nahi kar sakte:

```js
let name = "Hamid";

let name = "Ali"; // ❌ Error
```

Lekin reassignment allowed hai:

```js
let name = "Hamid";

name = "Ali"; // ✅
```

### Yaad rakhein

```js
let age = 20; // Declaration

age = 21; // Reassignment
```

Lekin:

```js
let age = 20;
let age = 21; // ❌ Redeclaration
```

---

# 4. `const`

`const` bhi ES6 ka keyword hai.

```js
const country = "Pakistan";
```

`const` variable ko dobara assign nahi kar sakte:

```js
const country = "Pakistan";

country = "India"; // ❌ Error
```

### `const` ko value dena zaroori hai

```js
const age; // ❌ Error
```

Correct:

```js
const age = 20;
```

### Redeclaration bhi allowed nahi

```js
const name = "Hamid";

const name = "Ali"; // ❌ Error
```

---

# 5. Reassignment vs Redeclaration

Ye difference bohot important hai.

### Reassignment

Existing variable ki value change karna:

```js
let score = 10;

score = 20;
```

Yahan variable dobara declare nahi hua.

### Redeclaration

Same variable ko dobara declare karna:

```js
let score = 10;

let score = 20; // ❌
```

### Simple Formula

```text
score = 20
      ↓
Reassignment

let score = 20
      ↓
Declaration / Redeclaration
```

---

# 6. Block Scope

Block ka matlab `{ }` ke andar ka area.

`let` aur `const` **block scoped** hain.

```js
{
  let city = "Karachi";

  console.log(city);
}
```

Lekin block ke bahar:

```js
{
  let city = "Karachi";
}

console.log(city); // ❌ Error
```

`const` bhi same tarah block scoped hai:

```js
{
  const country = "Pakistan";
}

console.log(country); // ❌ Error
```

---

# 7. `var` Block Scoped Nahi Hai

`var` block scoped nahi hota:

```js
{
  var city = "Karachi";
}

console.log(city);
```

Output:

```text
Karachi
```

Isi difference ki wajah se modern JavaScript mein `let` aur `const` ko prefer kiya jata hai.

---

# 8. Function Scope

`var` function scoped hota hai.

```js
function test() {
  var name = "Hamid";

  console.log(name);
}

test();
```

Lekin function ke bahar:

```js
function test() {
  var name = "Hamid";
}

console.log(name); // ❌ Error
```

`name` function ke andar hi available hai.

---

# 9. `let` in Loops

Loops mein `let` bohot useful hai:

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

`i` loop ke scope tak limited rehta hai.

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i); // ❌ Error
```

---

# 10. Hoisting

JavaScript declarations ko execution se pehle process karta hai. Is behavior ko **hoisting** kaha jata hai.

`var` ke saath:

```js
console.log(name);

var name = "Hamid";
```

Output:

```text
undefined
```

Conceptually:

```js
var name;

console.log(name);

name = "Hamid";
```

---

# 11. `let` aur `const` Hoisting

`let` aur `const` ko declaration se pehle access nahi kar sakte:

```js
console.log(age);

let age = 20;
```

❌ Error

```js
console.log(country);

const country = "Pakistan";
```

❌ Error

---

# 12. Temporal Dead Zone (TDZ)

`let` aur `const` declaration se pehle **Temporal Dead Zone (TDZ)** mein hote hain.

Simple rule:

> **`let` aur `const` ko declaration se pehle use nahi karna.**

```js
console.log(name); // ❌

let name = "Hamid";
```

---

# 13. `const` with Objects

`const` object ki properties ko change karne se nahi rokta.

```js
const user = {
  name: "Hamid",
  age: 20,
};

user.age = 21;

console.log(user.age);
```

Ye allowed hai.

Lekin poora object replace nahi kar sakte:

```js
const user = {
  name: "Hamid",
};

user = {
  name: "Ali",
}; // ❌
```

### Important

> `const` object ko completely immutable nahi banata.
> `const` sirf variable binding ko reassign hone se rokta hai.

---

# 14. `const` with Arrays

Array ke items change kar sakte hain:

```js
const fruits = ["Apple", "Banana"];

fruits.push("Mango");

console.log(fruits);
```

Ye allowed hai.

Lekin poora array replace nahi kar sakte:

```js
const fruits = ["Apple", "Banana"];

fruits = ["Mango", "Orange"]; // ❌
```

---

# 15. Kis Ko Kab Use Karein?

### `const`

Jab variable ko reassign nahi karna:

```js
const name = "Hamid";
const country = "Pakistan";
```

### `let`

Jab value future mein change hogi:

```js
let score = 0;

score = 10;
score = 20;
```

### `var`

Mostly old/legacy JavaScript code mein milta hai.

```js
var name = "Hamid";
```

> **Modern JavaScript mein normally `const` ko default choice rakhein aur value change hone par `let` use karein.**

---

# 16. Practical Example

```js
const username = "Hamid";
const country = "Pakistan";

let score = 0;

score = 10;

console.log(username);
console.log(country);
console.log(score);
```

Yahan:

- `username` change nahi hona → `const`
- `country` change nahi hona → `const`
- `score` change hona → `let`

---

# 17. Common Mistakes

### Mistake 1 — `const` ko reassign karna

```js
const age = 20;

age = 21; // ❌
```

Correct:

```js
let age = 20;

age = 21;
```

### Mistake 2 — `let` ko redeclare karna

```js
let city = "Karachi";

let city = "Lahore"; // ❌
```

Correct:

```js
let city = "Karachi";

city = "Lahore";
```

### Mistake 3 — `const` ko value ke baghair declare karna

```js
const name; // ❌
```

Correct:

```js
const name = "Hamid";
```

### Mistake 4 — Block scope bhool jana

```js
{
  let age = 20;
}

console.log(age); // ❌
```

---

# 18. Final Comparison

|                 | `var` | `let`        | `const`      |
| --------------- | ----- | ------------ | ------------ |
| Reassign        | ✅    | ✅           | ❌           |
| Redeclare       | ✅    | ❌           | ❌           |
| Block scoped    | ❌    | ✅           | ✅           |
| Function scoped | ✅    | Block scoped | Block scoped |
| Must initialize | ❌    | ❌           | ✅           |
| Modern use      | Rare  | Common       | Common       |

---

# 19. Quick Revision

```js
// var
var x = 10;
x = 20; // ✅
var x = 30; // ✅

// let
let y = 10;
y = 20; // ✅
let y = 30; // ❌

// const
const z = 10;
z = 20; // ❌
const z = 30; // ❌
```

### Sabse important formula

```text
const → value reassign nahi hogi

let → value change ho sakti hai

var → old style / legacy code
```

### 5 Rules Yaad Rakhein

1. **`const` ko default choice rakhein.**
2. **Value change karni ho to `let` use karein.**
3. **`var` old JavaScript mein zyada milta hai.**
4. **`let` aur `const` block scoped hain.**
5. **`let`/`const` ko declaration se pehle access nahi karna.**
