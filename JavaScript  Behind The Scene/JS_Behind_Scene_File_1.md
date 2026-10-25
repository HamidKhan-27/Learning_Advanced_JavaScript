# JavaScript Behind the Scenes
## File 1 — JavaScript Engine & Execution

**Topics Covered:**
1. JavaScript Engine
2. JavaScript Runtime
3. Execution Context
4. Global Execution Context (GEC)
5. Creation Phase & Execution Phase
6. Hoisting
7. `var`, `let`, `const` & Temporal Dead Zone (TDZ)
8. Function Execution Context (FEC)
9. Call Stack
10. Output-Based Examples

---

# PART 1 — JAVASCRIPT ENGINE

## 1. What Is a JavaScript Engine?

JavaScript Engine ek program hai jo JavaScript code ko process aur execute karta hai.

Jab hum JavaScript likhte hain, engine us code ko samajh kar execute karta hai.

**Examples:**
- Chrome mein V8 JavaScript Engine use hota hai.
- Node.js bhi V8 engine use karta hai.

```js
const name = "Hamid";
console.log(name);
```

**Output:**
```text
Hamid
```

Engine JavaScript instructions ko process karta hai aur `console.log()` ke zariye output display hota hai.

## 2. How Does a JavaScript Engine Work?

Modern JavaScript engines aam tor par code ko parse karte hain, internal representation banate hain aur execute karte hain. Performance improve karne ke liye code ko optimize bhi kar sakte hain.

Simplified flow:

1. **Parsing:** Engine code ki syntax check karta hai aur uski structure samajhta hai.
2. **Compilation/Preparation:** Engine code ko execute karne ke liye suitable internal form mein prepare karta hai.
3. **Execution:** Code run hota hai.
4. **Optimization:** Engine frequently used code ko tez execute karne ke liye optimize kar sakta hai.

Yeh ek simplified overview hai. Modern engines ke andar in steps ki implementation zyada complex hoti hai.

## 3. JavaScript Engine vs Browser

JavaScript Engine aur Browser ek hi cheez nahi hain.

**JavaScript Engine:**
- JavaScript code execute karta hai.
- Variables, functions aur expressions process karta hai.

**Browser:**
- JavaScript Engine provide karta hai.
- DOM ke zariye HTML elements access karne deta hai.
- Timers, browser events aur network requests jaisi Web APIs provide karta hai.

Example:

```js
console.log("Hello");

setTimeout(() => {
  console.log("Timer finished");
}, 1000);
```

`console.log()` JavaScript ki built-in functionality hai, jabke browser environment `setTimeout()` jaisi Web API provide karta hai.

**Important:** Browser JavaScript chalane ka ek environment hai, lekin JavaScript sirf browser mein nahi chalti. Node.js mein bhi JavaScript execute ho sakti hai.

---

# PART 2 — JAVASCRIPT RUNTIME & EXECUTION CONTEXT

## 4. What Is JavaScript Runtime?

Runtime se murad woh environment hai jahan JavaScript execute hoti hai.

Ismein JavaScript Engine aur host environment ki available facilities shamil hoti hain.

Examples:
- **Browser Runtime:** JavaScript Engine, DOM, timers, browser events aur doosri Web APIs.
- **Node.js Runtime:** JavaScript Engine ke saath Node.js ki APIs, jaise file system aur networking tools.

## 5. What Is an Execution Context?

Execution Context woh environment hai jismein JavaScript code execute hota hai.

Execution Context ko samajhne ke liye yeh dekho:

```js
const name = "Hamid";
const age = 20;

console.log(name);
console.log(age);
```

JavaScript ko code execute karte waqt variables aur doosri relevant information track karni hoti hai.

Execution Context conceptually in cheezon ko manage karta hai:
- Variables aur functions ki bindings.
- Current code ka execution environment.
- Outer environment ke saath connection, jahan se accessible variables mil sakte hain.

JavaScript ke execution contexts ki do important categories:

1. Global Execution Context (GEC)
2. Function Execution Context (FEC)

---

# PART 3 — GLOBAL EXECUTION CONTEXT

## 6. What Is Global Execution Context (GEC)?

Jab JavaScript program shuru hota hai, global-level code ko execute karne ke liye Global Execution Context create hota hai.

Example:

```js
const name = "Hamid";

function greet() {
  console.log("Hello");
}

console.log(name);
greet();
```

Is example mein:
- `name` global-level binding hai.
- `greet` global-level function declaration hai.
- `console.log(name)` aur `greet()` global-level code se execute hote hain.
- `greet()` call hone par ek Function Execution Context bhi create hota hai.

**Important:** Global Execution Context ka exact behavior script, module aur runtime ke hisaab se kuch farq rakh sakta hai.

## 7. Creation Phase

Execution Context create hote waqt JavaScript ko declarations aur bindings prepare karni hoti hain.

Simplified model mein:

- `var` declarations initialize ho kar `undefined` hoti hain.
- Function declarations available ho sakti hain before their written position.
- `let` aur `const` bindings create hoti hain, lekin initialization se pehle access nahi ki ja sakti. Is period ko Temporal Dead Zone kehte hain.

Example:

```js
console.log(score);

var score = 50;
```

**Output:**
```text
undefined
```

Kyun? Simplified model ke mutabiq `var score` ki binding execution se pehle initialize ho chuki hoti hai, lekin assignment `= 50` apni execution position par hoti hai.

## 8. Execution Phase

Execution phase mein JavaScript statements ko execute karti hai aur assignments perform hote hain.

```js
var score = 50;

console.log(score);

score = 80;

console.log(score);
```

**Output:**
```text
50
80
```

Step-by-step:
1. `score` ko `50` assign hota hai.
2. Pehla `console.log()` `50` print karta hai.
3. `score` ko `80` assign hota hai.
4. Doosra `console.log()` `80` print karta hai.

### Creation Phase vs Execution Phase

| Creation Phase | Execution Phase |
|---|---|
| Bindings prepare hoti hain. | Statements execute hoti hain. |
| `var` `undefined` se initialize hota hai. | Assignment se actual value milti hai. |
| Function declarations available ho sakti hain. | Function calls execute hote hain. |
| `let`/`const` TDZ mein reh sakte hain. | Initialization ke baad access possible hota hai. |

Yeh table ek learning model hai; JavaScript engine ki internal implementation is se zyada complex hai.

---

# PART 4 — HOISTING, VAR, LET, CONST & TDZ

## 9. What Is Hoisting?

Hoisting JavaScript ke declaration processing behavior ko samajhne ke liye use hone wala term hai.

Iska matlab yeh nahi ke JavaScript literally tumhara code utha kar file ke top par move kar deti hai.

Declarations ko execution se pehle process kiya jata hai, aur declaration ki type ke mutabiq behavior different hota hai.

## 10. Hoisting with `var`

```js
console.log(age);

var age = 20;

console.log(age);
```

**Output:**
```text
undefined
20
```

Samjho:

```js
var age = 20;
```

Ismein declaration aur assignment ka conceptual farq hai:

```js
var age; // declaration
age = 20; // assignment
```

Yeh sirf samjhane ke liye simplified equivalent hai.

`var` ki binding pehle initialize ho jati hai. Is liye assignment se pehle read karne par `undefined` milta hai, `ReferenceError` nahi.

## 11. Hoisting with `let`

```js
console.log(score);

let score = 100;
```

**Output:** `ReferenceError`

`let` ki binding execution se pehle create hoti hai, lekin declaration initialize hone tak access nahi ki ja sakti.

Yeh period **Temporal Dead Zone (TDZ)** kehlata hai.

```js
let score = 100;

console.log(score);
```

**Output:**
```text
100
```

## 12. Hoisting with `const`

```js
console.log(country);

const country = "Pakistan";
```

**Output:** `ReferenceError`

`const` bhi TDZ mein hota hai jab tak initialization na ho.

Is liye `const` declare karte waqt initial value dena zaroori hai.

```js
const country = "Pakistan";

console.log(country);
```

**Output:**
```text
Pakistan
```

Yeh invalid hai:

```js
const country;
```

Is se `SyntaxError` aata hai kyunke `const` declaration mein initializer required hai.

## 13. `var`, `let` & `const` Comparison

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| Scope | Function scope | Block scope | Block scope |
| Reassignment | Allowed | Allowed | Not allowed |
| Same scope mein redeclaration | Generally allowed | Not allowed | Not allowed |
| Initialization se pehle access | `undefined` (if declared) | TDZ error | TDZ error |
| Initial value required | No | No | Yes |

**Reassignment** ka matlab existing variable ko nayi value dena.

```js
let age = 20;
age = 21;

console.log(age);
```

Output: `21`

**Redeclaration** ka matlab same scope mein usi variable name ko dobara declare karna.

```js
let age = 20;
// let age = 21; // SyntaxError in the same scope
```

### Important: `const` and Objects

`const` ka matlab yeh nahi ke object ki properties change nahi ho sakti.

```js
const user = {
  name: "Hamid"
};

user.name = "Ali";

console.log(user.name);
```

**Output:**
```text
Ali
```

Object ki property change ho sakti hai, lekin `user` binding ko kisi naye object se reassign nahi kar sakte.

```js
const user = {
  name: "Hamid"
};

// user = { name: "Ali" }; // TypeError
```

---

# PART 5 — FUNCTION EXECUTION CONTEXT

## 14. What Is Function Execution Context (FEC)?

Jab koi function call hota hai, us function ki execution ke liye Function Execution Context create hota hai.

Function ko sirf define karne se woh execute nahi hota.

```js
function greet() {
  console.log("Hello Hamid");
}
```

Is code mein function define hua hai, lekin call nahi hua; is liye output nahi aayega.

Ab function call karo:

```js
function greet() {
  console.log("Hello Hamid");
}

greet();
```

**Output:**
```text
Hello Hamid
```

Function call hone par uski execution start hoti hai.

Function ke parameters, local variables aur execution se related information uske execution context ke saath manage hoti hai.

## 15. Function Execution Example

```js
const name = "Hamid";

function greet() {
  const message = "Hello";
  console.log(message, name);
}

greet();
```

**Output:**
```text
Hello Hamid
```

Yahan:
- `name` global scope mein hai.
- `message` function ke andar local variable hai.
- `greet()` call hone par Function Execution Context create hota hai.
- Function `name` ko outer scope se access kar sakta hai.

## 16. Nested Function Calls

```js
function first() {
  console.log("A");

  second();

  console.log("B");
}

function second() {
  console.log("C");
}

first();
```

**Output:**
```text
A
C
B
```

Step-by-step:

1. `first()` call hota hai.
2. `"A"` print hota hai.
3. `second()` call hota hai.
4. `"C"` print hota hai.
5. `second()` finish hota hai.
6. Control `first()` mein wapas aata hai.
7. `"B"` print hota hai.

**Golden rule:** Jab nested function call finish hota hai, execution calling function mein wapas aati hai, jahan se call kiya gaya tha.

---

# PART 6 — CALL STACK

## 17. What Is the Call Stack?

Call Stack ek mechanism hai jo track karta hai ke kaun se function calls abhi execute ho rahe hain.

Yeh **LIFO** principle follow karta hai:

**Last In, First Out**

Yani jo function sab se baad stack mein enter karta hai, woh pehle complete hota hai.

Example:

```js
function one() {
  console.log("One");

  two();

  console.log("One finished");
}

function two() {
  console.log("Two");
}

one();
```

**Output:**
```text
One
Two
One finished
```

Simplified stack flow:

1. Global code execute hota hai.
2. `one()` call hota hai; `one` ka execution context stack par aata hai.
3. `two()` call hota hai; `two` ka context stack ke top par aata hai.
4. `two()` finish hota hai aur uska context remove hota hai.
5. `one()` dobara continue hota hai.
6. `one()` finish hota hai.
7. Global code continue karta hai.

## 18. Call Stack Diagram

```text
During two() execution:

        ┌──────────────┐
        │   two()      │  ← Top of stack
        ├──────────────┤
        │   one()      │
        ├──────────────┤
        │ Global Code  │
        └──────────────┘
```

Jab `two()` finish hota hai, uska context stack se remove ho jata hai.

```text
After two() finishes:

        ┌──────────────┐
        │   one()      │  ← Top of stack
        ├──────────────┤
        │ Global Code  │
        └──────────────┘
```

Yeh conceptual diagram hai; actual engine internals is se zyada complex hain.

## 19. What Is a Stack Overflow?

Agar functions ek doosre ko repeatedly call karte rahen aur calls ki depth bohot zyada ho jaye, to call stack ki limit exceed ho sakti hai.

```js
function repeat() {
  repeat();
}

repeat();
```

Is example mein function apne aap ko bina kisi stopping condition ke call karta rehta hai.

Aam tor par is se error aata hai:

```text
RangeError: Maximum call stack size exceeded
```

Isay stack overflow kehte hain.

Infinite recursion ko rokne ke liye aksar base condition zaroori hoti hai.

```js
function countDown(number) {
  if (number <= 0) {
    return;
  }

  console.log(number);
  countDown(number - 1);
}

countDown(3);
```

**Output:**
```text
3
2
1
```

Jab `number <= 0` hota hai, function return kar deta hai aur recursion ruk jati hai.

---

# PART 7 — QUICK REVISION

## 20. Important Concepts at a Glance

| Concept | Yaad rakhne wali baat |
|---|---|
| JavaScript Engine | JavaScript code process aur execute karta hai. |
| Runtime | Engine aur host environment ki facilities. |
| Execution Context | Code execute karne ka environment. |
| GEC | Global-level code ka execution context. |
| Creation Phase | Bindings aur declarations prepare hoti hain. |
| Execution Phase | Statements run aur assignments perform hote hain. |
| Hoisting | Declaration processing ko samajhne ka term; code literally move nahi hota. |
| `var` | Initialization se pehle read karne par `undefined` mil sakta hai. |
| `let` / `const` | Initialization se pehle TDZ ki wajah se access error hota hai. |
| FEC | Function call hone par function execution context create hota hai. |
| Call Stack | Active function calls ko track karta hai. |
| LIFO | Last In, First Out. |
| Stack Overflow | Call stack ki limit exceed ho jana. |

## 21. Output Tracing Checklist

JavaScript ka output predict karte waqt yeh steps follow karo:

1. Dekho code mein declarations kis type ki hain: `var`, `let`, ya `const`.
2. Check karo variable ko initialization se pehle access to nahi kiya ja raha.
3. Global aur function-level code ko alag samjho.
4. Har function call ko trace karo.
5. Nested calls mein dekho function kahan return karta hai.
6. Call Stack mein last called function pehle complete hota hai.
7. Error aane par us point ke baad ka normal execution ruk sakta hai, jab tak error handle na kiya gaya ho.

## Final Summary

Is file ka central idea yeh hai ke JavaScript code sirf line-by-line chalne ka naam nahi. Engine declarations ko process karta hai, execution contexts code ke environment ko manage karte hain, aur Call Stack function calls ka order track karta hai.

In concepts ko samajh lene se hoisting, scope, closures aur asynchronous JavaScript ko aage samajhna kaafi asaan ho jata hai.

**Next File:** JavaScript Behind the Scenes — File 2: Memory, Scope & Closures.
