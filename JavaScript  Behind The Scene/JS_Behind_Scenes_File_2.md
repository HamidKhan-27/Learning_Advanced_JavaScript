# JavaScript Behind the Scenes
## File 2 — Memory, Scope & Closures

**Topics Covered:**
1. JavaScript Memory
2. Stack vs Heap
3. Primitive Values vs Object References
4. `const` and Object Mutation
5. Garbage Collection
6. Scope and Types of Scope
7. Lexical Environment
8. Scope Chain
9. Lexical Scope
10. Closures
11. Output-Based Examples
12. Quick Revision

---

# PART 1 — JAVASCRIPT MEMORY

## 1. What Is Memory in JavaScript?

Jab JavaScript program execute hota hai, to usay values, variables aur objects ko manage karne ke liye memory chahiye hoti hai.

Example:

```js
const name = "Hamid";
let age = 20;

const user = {
  city: "Karachi",
  country: "Pakistan"
};

console.log(name);
console.log(age);
console.log(user.city);
```

**Output:**

```text
Hamid
20
Karachi
```

JavaScript ko in values ko manage karna hota hai jab program execute ho raha hota hai.

Memory ko samajhne ke liye hum do useful concepts use karte hain:

- Stack
- Heap

**Important:** Stack aur Heap memory ko samajhne ke conceptual models hain. Har variable ya value ki physical memory location har JavaScript engine mein exactly ek jaisi nahi hoti.

---

# PART 2 — STACK VS HEAP

## 2. What Is Stack Memory?

Stack ko samajhne ke liye execution contexts aur function calls ka concept yaad rakho.

Call Stack active function calls ko track karta hai. Function call hone par uski execution se related information manage hoti hai, aur function finish hone par uski call stack entry remove ho jati hai.

Example:

```js
function greet() {
  const message = "Hello";
  console.log(message);
}

greet();
console.log("Finished");
```

**Output:**

```text
Hello
Finished
```

Jab `greet()` execute hota hai, uski function call active hoti hai. Function finish hone par control wapas caller ko milta hai.

## 3. What Is Heap Memory?

Heap ek memory area ka conceptual naam hai jo objects aur dynamically allocated data ke liye commonly use hota hai.

Example:

```js
const user = {
  name: "Hamid",
  age: 20
};

console.log(user.name);
```

**Output:**

```text
Hamid
```

Object ko access karne ke liye `user` variable use hota hai. Object aur uske reference ko samajhna important hai, khaas tor par jab hum ek object ko multiple variables mein assign karte hain.

## 4. Stack vs Heap Comparison

| Stack | Heap |
|---|---|
| Call Stack active function calls track karta hai. | Heap objects aur dynamically allocated data ke liye commonly use hota hai. |
| Function calls ka order samajhne mein madad karta hai. | Objects aur unke references ko samajhne mein madad karta hai. |
| Function return hone par uski active call finish hoti hai. | Object ki memory tab reclaim ho sakti hai jab woh unreachable ho jaye. |

**Important:** Yeh simplified learning model hai. Iska matlab yeh nahi ke har primitive value hamesha stack mein aur har object hamesha heap mein hi store hota hai.

---

# PART 3 — PRIMITIVE VALUES VS OBJECT REFERENCES

## 5. What Are Primitive Data Types?

JavaScript ke primitive data types yeh hain:

1. String
2. Number
3. Boolean
4. Undefined
5. Null
6. BigInt
7. Symbol

Example:

```js
const name = "Hamid";
const age = 20;
const isStudent = true;

console.log(name);
console.log(age);
console.log(isStudent);
```

**Output:**

```text
Hamid
20
true
```

Primitives ko assign karte waqt value copy hone ka model use karna bohot helpful hai.

## 6. Primitive Value Copy

```js
let a = 10;
let b = a;

b = 20;

console.log(a);
console.log(b);
```

**Output:**

```text
10
20
```

Step-by-step:

1. `a` mein `10` hai.
2. `b = a` se `a` ki current value copy hoti hai.
3. `b = 20` se sirf `b` ki value change hoti hai.
4. `a` ab bhi `10` hai.

Yani primitive value copy karne ke baad dono variables independently change ho sakte hain.

## 7. Objects and References

Objects ke saath behavior different hota hai.

```js
const user1 = {
  name: "Hamid"
};

const user2 = user1;

user2.name = "Ali";

console.log(user1.name);
console.log(user2.name);
```

**Output:**

```text
Ali
Ali
```

Aisa kyun hua?

- `user1` ek object ko refer karta hai.
- `user2 = user1` karne se object ki independent copy nahi banti.
- Dono variables usi object ko refer karte hain.
- `user2.name = "Ali"` usi shared object ki property change karta hai.

Conceptually:

```text
user1 ─────┐
           ├────> { name: "Ali" }
user2 ─────┘
```

**Golden rule:** Object ko doosre variable mein assign karne se reference copy hota hai, automatically deep copy nahi banti.

## 8. Separate Objects Are Independent

```js
const user1 = {
  name: "Hamid"
};

const user2 = {
  name: "Hamid"
};

user2.name = "Ali";

console.log(user1.name);
console.log(user2.name);
```

**Output:**

```text
Hamid
Ali
```

Yahan dono objects separately create hue hain. Is liye `user2` ki property change karne se `user1` ka object change nahi hota.

## 9. `const` with Objects

`const` ka matlab hai variable binding ko kisi doosri value ya object se reassign nahi kar sakte.

Lekin object ki properties mutate ho sakti hain.

```js
const user = {
  name: "Hamid",
  age: 20
};

user.name = "Ali";
user.age = 21;

console.log(user.name);
console.log(user.age);
```

**Output:**

```text
Ali
21
```

Yeh allowed hai kyunke object ki properties change ho rahi hain, `user` binding reassign nahi ho rahi.

Yeh allowed nahi:

```js
const user = {
  name: "Hamid"
};

user = {
  name: "Ali"
};
```

Is se `TypeError` aata hai kyunke `user` ko dobara assign karne ki koshish ki gayi.

**Yaad rakho:**

- `const` binding ko reassign nahi kar sakte.
- `const` object ki properties change ho sakti hain.
- Agar object ko immutable banana ho to additional techniques chahiye; `const` akela object ko immutable nahi banata.

---

# PART 4 — GARBAGE COLLECTION

## 10. What Is Garbage Collection?

JavaScript engine memory management karta hai. Jab koi object program se reachable nahi rehta, to garbage collector uski memory ko future mein reclaim kar sakta hai.

Example:

```js
let user = {
  name: "Hamid"
};

user = null;
```

Pehle `user` object ko refer karta hai. Phir `user = null` se woh reference hata diya jata hai.

Lekin iska matlab yeh nahi ke object ki memory usi waqt lazmi free ho jati hai.

Garbage collection ka exact time engine decide karta hai.

**Important points:**

- Unreachable objects garbage collection ke liye eligible ho sakte hain.
- Memory immediately reclaim hone ki guarantee nahi hoti.
- Agar kisi aur variable ya closure ke paas object ka reachable reference hai, to object abhi bhi use ho sakta hai.

---

# PART 5 — SCOPE

## 11. What Is Scope?

Scope define karta hai ke program ke kis area mein variable ko access kiya ja sakta hai.

JavaScript mein commonly yeh scopes samjhe jate hain:

1. Global Scope
2. Function Scope
3. Block Scope

## 12. Global Scope

Jo variable global-level par declare hota hai, woh apne relevant global environment mein accessible hota hai, subject to script/module rules.

```js
const name = "Hamid";

function greet() {
  console.log(name);
}

greet();
console.log(name);
```

**Output:**

```text
Hamid
Hamid
```

`name` global-level par declared hai, is liye function usay outer scope se access kar sakta hai.

## 13. Function Scope

Function ke andar declared local variable us function ke andar accessible hota hai.

```js
function greet() {
  const message = "Hello";
  console.log(message);
}

greet();
```

**Output:**

```text
Hello
```

Lekin:

```js
function greet() {
  const message = "Hello";
}

greet();
console.log(message);
```

Yahan `ReferenceError` aayega kyunke `message` function ke bahar accessible nahi hai.

## 14. Block Scope

Block aam tor par curly braces `{}` ke andar ka code hota hai, jaise `if`, `for` ya standalone block.

`let` aur `const` block-scoped hote hain.

```js
if (true) {
  let age = 20;
  const name = "Hamid";

  console.log(age);
  console.log(name);
}
```

**Output:**

```text
20
Hamid
```

Lekin block ke bahar:

```js
if (true) {
  let age = 20;
}

console.log(age);
```

**Result:** `ReferenceError`

Kyunke `age` sirf `if` block ke andar accessible hai.

## 15. `var` vs `let` in Blocks

`var` block-scoped nahi hota. Yeh function-scoped hota hai, ya agar function ke bahar declare ho to relevant global/script environment mein binding create karta hai.

```js
if (true) {
  var a = 10;
  let b = 20;
}

console.log(a);
console.log(b);
```

**Output:**

```text
10
ReferenceError
```

Explanation:

- `a` ko `var` se declare kiya gaya, is liye woh `if` block tak limited nahi hai.
- `b` ko `let` se declare kiya gaya, is liye woh sirf block ke andar accessible hai.
- `console.log(b)` par error aata hai.

## 16. Scope Comparison

| Scope | `var` | `let` / `const` |
|---|---|---|
| Function ke andar | Function-scoped | Function ke andar declare hon to us block-scoped region tak |
| `if` block ke andar | Block se bahar bhi accessible ho sakta hai | Block ke bahar accessible nahi |
| Outer variable access | Outer accessible bindings ko use kar sakta hai | Outer accessible bindings ko use kar sakta hai |

**Important:** Scope decide karta hai ke variable kahan accessible hai. Hoisting aur TDZ decide karte hain ke variable apni scope ke andar kis waqt access kiya ja sakta hai.

---

# PART 6 — LEXICAL ENVIRONMENT & SCOPE CHAIN

## 17. What Is a Lexical Environment?

Lexical Environment ek conceptual structure hai jo current scope ki bindings aur outer lexical environment se connection ko samajhne mein madad karta hai.

Simple alfaaz mein, JavaScript ko pata hota hai:

- Current scope mein kaun se variables available hain.
- Outer scope se kaun se variables access kiye ja sakte hain.

Example:

```js
const country = "Pakistan";

function showCountry() {
  const city = "Karachi";

  console.log(city);
  console.log(country);
}

showCountry();
```

**Output:**

```text
Karachi
Pakistan
```

`city` current function scope mein milta hai, jabke `country` outer scope se milta hai.

## 18. What Is the Scope Chain?

Jab JavaScript kisi variable ko access karti hai, to woh current lexical environment mein us variable ki binding dhoondti hai.

Agar variable wahan na mile, to outer environment mein search karti hai. Yeh process outer environments ke through continue hota hai.

Isay Scope Chain kehte hain.

Example:

```js
const a = 10;

function outer() {
  const b = 20;

  function inner() {
    const c = 30;

    console.log(a);
    console.log(b);
    console.log(c);
  }

  inner();
}

outer();
```

**Output:**

```text
10
20
30
```

`inner()`:
- `c` apne current scope mein find karta hai.
- `b` outer function ke scope mein find karta hai.
- `a` global scope mein find karta hai.

Conceptual structure:

```text
inner scope: c
     ↓
outer scope: b
     ↓
global scope: a
```

Agar variable kisi accessible scope mein na mile, to `ReferenceError` aata hai.

## 19. Inner Scope vs Outer Scope

Inner function outer variables ko access kar sakta hai, lekin outer function inner function ke local variables ko directly access nahi kar sakta.

```js
function outer() {
  const outerValue = "Outer";

  function inner() {
    const innerValue = "Inner";

    console.log(outerValue);
    console.log(innerValue);
  }

  inner();

  console.log(outerValue);
  console.log(innerValue);
}

outer();
```

**Output before the error:**

```text
Outer
Inner
Outer
```

Phir `ReferenceError` aata hai kyunke `innerValue` sirf `inner()` ke scope mein accessible hai.

---

# PART 7 — LEXICAL SCOPE

## 20. What Is Lexical Scope?

Lexical scope ka matlab hai ke function ka outer-variable access us jagah par depend karta hai jahan function **define** hua hai, na ke us jagah par jahan se usay call kiya gaya hai.

Example:

```js
const name = "Hamid";

function showName() {
  console.log(name);
}

function test() {
  const name = "Ali";
  showName();
}

test();
```

**Output:**

```text
Hamid
```

Kyun?

1. `showName()` global scope mein define hua.
2. Uska lexical outer environment global scope hai.
3. `test()` ke andar `name` ki value `"Ali"` hai.
4. Lekin `showName()` apne lexical scope chain ko follow karta hai.
5. Is liye woh global `"Hamid"` print karta hai.

**Golden rule:** Function ka lexical scope uski definition ki location se decide hota hai, call location se nahi.

---

# PART 8 — CLOSURES

## 21. What Is a Closure?

Closure tab samajh aata hai jab ek inner function apne outer lexical environment ki bindings ko access karta rahe, chahe outer function ki execution finish ho chuki ho.

Example:

```js
function outer() {
  const message = "Hello from outer";

  function inner() {
    console.log(message);
  }

  return inner;
}

const result = outer();

result();
```

**Output:**

```text
Hello from outer
```

Step-by-step:

1. `outer()` call hota hai.
2. `message` variable create hota hai.
3. `inner` function return hota hai.
4. `outer()` ki execution finish hoti hai.
5. `result()` se `inner()` call hota hai.
6. `inner()` ab bhi `message` ko access kar sakta hai.

Inner function apne lexical environment se connection retain karta hai. Isi behavior ko closure ke concept se samjha jata hai.

## 22. Closure with a Counter

Closures ka ek practical use private state maintain karna hai.

```js
function createCounter() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

const counter = createCounter();

counter();
counter();
counter();
```

**Output:**

```text
1
2
3
```

Kyun?

- `createCounter()` ek function return karta hai.
- Returned function `count` ko access karta rehta hai.
- Har call par wahi retained `count` increment hota hai.
- `count` ko direct global variable nahi banaya gaya.

Agar hum ek aur counter banayen:

```js
const counter1 = createCounter();
const counter2 = createCounter();

counter1();
counter1();
counter2();
counter1();
counter2();
```

**Output:**

```text
1
2
1
3
2
```

Har `createCounter()` call apna separate `count` create karta hai. Is liye `counter1` aur `counter2` ka state alag hai.

## 23. Closure Does Not Mean a Frozen Copy

Closure ko aise mat samjho ke function variable ki sirf ek fixed value ki copy save karta hai. Closure retained lexical binding ko access kar sakta hai, aur agar woh binding change ho to updated value bhi nazar aa sakti hai.

```js
function outer() {
  let score = 10;

  return function () {
    score += 5;
    console.log(score);
  };
}

const increaseScore = outer();

increaseScore();
increaseScore();
increaseScore();
```

**Output:**

```text
15
20
25
```

Inner function same retained `score` binding ko update kar raha hai.

---

# PART 9 — COMMON MISTAKES

## 24. Mistake: Objects Automatically Copy Ho Jate Hain

```js
const first = { score: 10 };
const second = first;

second.score = 99;

console.log(first.score);
```

Output: `99`

Dono variables same object ko refer karte hain.

## 25. Mistake: `const` Object Immutable Hota Hai

```js
const user = { name: "Hamid" };

user.name = "Ali";

console.log(user.name);
```

Output: `Ali`

`const` binding ko protect karta hai, object ki properties ko automatically immutable nahi banata.

## 26. Mistake: Outer Function Inner Variable Access Kar Sakta Hai

```js
function outer() {
  function inner() {
    const secret = "Private";
  }

  inner();

  console.log(secret);
}

outer();
```

`ReferenceError` aayega kyunke `secret` inner function ka local variable hai.

## 27. Mistake: Function Call Location Scope Decide Karti Hai

```js
const value = "Global";

function show() {
  console.log(value);
}

function run() {
  const value = "Local";
  show();
}

run();
```

Output: `Global`

Function `show` global scope mein define hua tha, is liye woh global `value` resolve karta hai.

---

# PART 10 — QUICK REVISION

## 28. Important Concepts at a Glance

| Concept | Yaad rakhne wali baat |
|---|---|
| Memory | Values aur objects ko manage karne ke liye use hoti hai. |
| Stack | Call Stack active function calls track karta hai. |
| Heap | Objects aur dynamic data ke liye commonly used conceptual memory area. |
| Primitive Assignment | Primitive value ki copy milti hai. |
| Object Assignment | Reference copy hota hai; object automatically clone nahi hota. |
| `const` Object | Properties mutate ho sakti hain; binding reassign nahi ho sakti. |
| Garbage Collection | Unreachable memory reclaim ho sakti hai; timing guaranteed nahi. |
| Scope | Batata hai variable kahan accessible hai. |
| Function Scope | Function ke local variables function tak limited hote hain. |
| Block Scope | `let` aur `const` block-scoped hote hain. |
| Lexical Environment | Current bindings aur outer environment connection ka conceptual model. |
| Scope Chain | Variable ko current scope se outer scopes tak search karna. |
| Lexical Scope | Function ka outer scope uski definition ki location se decide hota hai. |
| Closure | Function apne outer lexical environment ki bindings ko retain karke access kar sakta hai. |

## 29. Output Tracing Checklist

Code solve karte waqt:

1. Dekho primitive value hai ya object.
2. Object assignment par check karo ke references shared hain ya separate.
3. `const` mein property mutation aur binding reassignment ka farq samjho.
4. Variable ka scope identify karo: global, function ya block.
5. Variable ko current scope mein dhoondo, phir outer scope mein.
6. Function kahan define hua hai, yeh check karo.
7. Closure mein dekho inner function kaunsi outer bindings access karta hai.
8. Function return hone ke baad bhi closure ke through retained bindings accessible ho sakti hain.

## Final Summary

Memory aur Scope JavaScript ke bohot important concepts hain.

- Stack aur Heap memory ko samajhne ke useful models hain.
- Primitive assignment value copy karta hai; object assignment reference copy karta hai.
- Scope variable ki accessibility decide karta hai.
- Scope Chain outer environments mein variables search karti hai.
- Lexical Scope function ki definition ki location par depend karta hai.
- Closure inner function ko outer lexical environment ki bindings access karne deta hai.

**Next File:** JavaScript Behind the Scenes — File 3: Asynchronous JavaScript & Revision.
