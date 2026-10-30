# JavaScript Behind the Scenes

## File 3 — Asynchronous JavaScript & Revision

**Topics Covered:**

1. Synchronous JavaScript
2. Asynchronous JavaScript
3. Blocking vs Non-Blocking Code
4. Callbacks
5. Web APIs
6. Callback Queue / Task Queue
7. Event Loop
8. Promise Microtask Queue
9. Timer vs Promise Execution Order
10. Combined Execution Flow
11. Output-Based Examples
12. Final Revision Checklist

---

# PART 1 — SYNCHRONOUS JAVASCRIPT

## 1. What Is Synchronous JavaScript?

Synchronous JavaScript mein statements aam tor par ek ke baad ek execute hoti hain. Agli statement tab execute hoti hai jab current statement complete ho jaye.

Example:

```js
console.log("First");
console.log("Second");
console.log("Third");
```

**Output:**

```text
First
Second
Third
```

JavaScript pehle `"First"` print karti hai, phir `"Second"` aur aakhir mein `"Third"`.

## 2. Synchronous Function Calls

```js
function first() {
  console.log("Inside first");
}

function second() {
  console.log("Inside second");
}

first();
second();

console.log("Finished");
```

**Output:**

```text
Inside first
Inside second
Finished
```

`first()` complete hota hai, phir `second()` execute hota hai, aur uske baad `"Finished"` print hota hai.

## 3. Blocking Code

Agar koi synchronous operation bohot zyada waqt leti hai, to woh us waqt JavaScript thread par doosre kaam ko delay kar sakti hai.

Example:

```js
console.log("Start");

let total = 0;

for (let i = 0; i < 1000000; i++) {
  total += i;
}

console.log("Calculation finished");
console.log("End");
```

**Output ka order:**

```text
Start
Calculation finished
End
```

Loop complete hone tak aage ki statements wait karti hain.

**Important:** Har loop problematic nahi hota. Masla tab hota hai jab synchronous work itna lamba ho ke page ya application response dene mein delay kare.

---

# PART 2 — ASYNCHRONOUS JAVASCRIPT

## 4. What Is Asynchronous JavaScript?

Asynchronous operations aise kaam karne deti hain jo baad mein complete hote hain, jabke JavaScript apna doosra available code execute kar sakti hai.

Examples:

- `setTimeout()`
- Network requests, jaise `fetch()`
- Browser events, jaise button clicks

Example:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer finished");
}, 2000);

console.log("End");
```

**Output ka order:**

```text
Start
End
Timer finished
```

Explanation:

1. `"Start"` print hota hai.
2. `setTimeout()` timer schedule karne ke liye use hota hai.
3. JavaScript agla statement execute karti hai.
4. `"End"` print hota hai.
5. Timer callback baad mein execute hota hai jab woh ready ho aur event loop usay run karne ka mauqa de.

## 5. Does `setTimeout()` Run Exactly After Two Seconds?

Nahi. `setTimeout(callback, 2000)` ka matlab yeh nahi ke callback bilkul 2000 milliseconds par execute hone ki guarantee hai.

Iska matlab hai ke callback specified delay se pehle execute nahi hona chahiye, lekin woh baad mein execute ho sakta hai.

Delay ki wajah:

- Main thread busy ho.
- Synchronous code abhi execute ho raha ho.
- Event loop mein pehle se doosre tasks hon.

**Golden rule:** Timer delay minimum waiting period jaisa hai, exact execution time ki guarantee nahi.

## 6. Zero-Millisecond Timer

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

**Output:**

```text
A
C
B
```

`setTimeout(..., 0)` ka matlab callback ko isi waqt synchronous code ke darmiyan execute karna nahi hai. Callback baad mein task ke taur par execute hota hai.

---

# PART 3 — CALLBACKS

## 7. What Is a Callback Function?

Callback ek function hai jo doosre function ko argument ke taur par pass kiya jata hai, taake doosra function usay call kar sake.

Callback synchronous bhi ho sakta hai aur asynchronous bhi.

Example — synchronous callback:

```js
function greet(callback) {
  console.log("Hello");

  callback();
}

function sayBye() {
  console.log("Goodbye");
}

greet(sayBye);
```

**Output:**

```text
Hello
Goodbye
```

Yahan `sayBye` ko callback ke taur par pass kiya gaya hai.

## 8. Callback with `setTimeout()`

```js
function welcome() {
  console.log("Welcome!");
}

setTimeout(welcome, 1000);
```

Yahan:

- `welcome` callback function hai.
- `setTimeout` timer schedule karta hai.
- Callback baad mein execute hota hai jab woh ready ho aur event loop usay process kare.

**Important difference:**

```js
setTimeout(welcome, 1000);
```

Function ko reference pass karta hai.

```js
setTimeout(welcome(), 1000);
```

Yeh `welcome()` ko foran call karta hai aur uske return value ko `setTimeout` ko pass karta hai. Yeh pehle wale code jaisa nahi hai.

---

# PART 4 — WEB APIs

## 9. What Are Web APIs?

Browser JavaScript ke ilawa additional features provide karta hai jinhein Web APIs kaha jata hai.

Examples:

- `setTimeout()`
- DOM methods, jaise `document.querySelector()`
- Browser events, jaise click aur keyboard events
- `fetch()` for network requests

Example:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timer callback");
}, 1000);

console.log("End");
```

Browser timer ko manage karne mein madad karta hai. JavaScript ka synchronous code apni execution continue karta hai. Jab timer callback ready ho jata hai, woh task queue ke through execute hone ke liye wait kar sakta hai.

**Important:** Web APIs browser environment ka hissa hain. Node.js mein bhi timers aur networking jaisi facilities hoti hain, lekin unki implementation browser Web APIs se different ho sakti hai.

---

# PART 5 — CALLBACK QUEUE & EVENT LOOP

## 10. What Is the Callback Queue?

Jab asynchronous callback execute hone ke liye ready ho jata hai, to woh task ke taur par wait kar sakta hai jab tak JavaScript thread usay execute karne ke liye available na ho.

Timer callbacks jaise tasks ko samajhne ke liye hum **Callback Queue** ya **Task Queue** ka simplified model use karte hain.

Example:

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

**Output:**

```text
A
C
B
```

Timer callback current synchronous code ke beech mein nahi chalta. Pehle synchronous statements complete hoti hain.

## 11. What Is the Event Loop?

Event Loop JavaScript runtime mein scheduling process ka important hissa hai. Yeh check karne mein madad karta hai ke ready tasks kab execute ho sakte hain.

Simplified process:

1. Synchronous JavaScript execute hoti hai.
2. Timer ya doosre asynchronous operations apni completion ka wait karte hain.
3. Ready callbacks apni relevant queue mein wait karte hain.
4. Current synchronous execution complete hoti hai.
5. Event loop ready work ko execution ke liye aage laata hai.

**Yaad rakho:** Event Loop khud tumhara callback execute nahi karta; callback JavaScript thread par execute hota hai jab runtime usay schedule karta hai aur thread available hota hai.

## 12. Event Loop Flow

```text
Synchronous JavaScript
         |
         v
   Call Stack runs
         |
         v
Async operation becomes ready
         |
         v
Callback waits in a task queue
         |
         v
Current synchronous work finishes
         |
         v
Event Loop schedules ready work
         |
         v
Callback executes
```

Yeh learning ke liye simplified diagram hai. Browser event loop mein different task sources aur additional scheduling rules bhi hote hain.

---

# PART 6 — PROMISES & MICROTASK QUEUE

## 13. What Is a Promise?

Promise JavaScript ka object hai jo asynchronous operation ke eventual success ya failure ko represent karta hai.

Promise ki teen states hoti hain:

1. **Pending:** Operation abhi settle nahi hui.
2. **Fulfilled:** Operation successful ho gayi.
3. **Rejected:** Operation fail ho gayi.

Example:

```js
const result = Promise.resolve("Success");

result.then((value) => {
  console.log(value);
});
```

**Output:**

```text
Success
```

`then()` callback fulfilled promise ke liye microtask ke taur par schedule hota hai; woh current synchronous code ke darmiyan immediately execute nahi hota.

## 14. What Is the Microtask Queue?

Promise ke `.then()`, `.catch()` aur `.finally()` callbacks aam tor par microtasks ke taur par schedule hote hain.

Browser JavaScript ke simplified model mein:

- Current synchronous JavaScript complete hoti hai.
- Pending microtasks process hoti hain.
- Uske baad ordinary timer tasks process ho sakte hain, subject to event loop scheduling rules.

**Important:** Promise microtasks aur timer tasks ko ek hi queue samajhna ghalat hai.

## 15. Promise vs Timer

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

**Output:**

```text
A
D
C
B
```

Step-by-step:

1. `"A"` synchronous code mein print hota hai.
2. Timer callback schedule hota hai.
3. Promise `.then()` callback microtask ke taur par schedule hota hai.
4. `"D"` synchronous code mein print hota hai.
5. Synchronous code finish hone par Promise microtask `"C"` execute hoti hai.
6. Uske baad timer callback `"B"` execute hota hai.

**Golden rule for typical browser output questions:** Current synchronous code first, then Promise microtasks, then ready timer tasks.

---

# PART 7 — COMBINED EXECUTION FLOW

## 16. Function Calls + Timer + Global Code

```js
function greet() {
  console.log("Hello");
}

console.log("Start");

setTimeout(greet, 0);

console.log("End");
```

**Output:**

```text
Start
End
Hello
```

Reason:

- `"Start"` synchronous hai.
- Timer callback schedule hota hai.
- `"End"` synchronous hai.
- `greet()` baad mein execute hota hai.

## 17. Scope + Closure + Timer

```js
function outer() {
  const name = "Ali";

  function inner() {
    console.log(name);
  }

  setTimeout(inner, 0);

  console.log("Outer finished");
}

outer();

console.log("Global finished");
```

**Output:**

```text
Outer finished
Global finished
Ali
```

Explanation:

1. `outer()` execute hota hai.
2. `name` ki value `"Ali"` hoti hai.
3. `inner` function define hota hai aur `name` ko access kar sakta hai.
4. `setTimeout()` `inner` callback ko schedule karta hai.
5. `"Outer finished"` print hota hai.
6. `outer()` finish hota hai.
7. `"Global finished"` print hota hai.
8. Timer callback execute hota hai aur `"Ali"` print karta hai.

`inner` apne lexical environment se connection retain karta hai. Is liye outer function finish hone ke baad bhi `name` accessible rehta hai.

## 18. Hoisting + Function Scope + Timer

```js
var number = 5;

function test() {
  console.log(number);

  var number = 10;

  setTimeout(() => {
    console.log(number);
  }, 0);

  console.log(number);
}

test();

console.log(number);
```

**Output:**

```text
undefined
10
5
10
```

Explanation:

1. Global `number` ki value `5` hai.
2. `test()` ke andar bhi local `var number` declare hai.
3. Function scope ke andar local `var number` declaration process hone ki wajah se pehla `console.log(number)` local binding ko read karta hai, jo abhi `undefined` hai.
4. Local `number` ko `10` assign hota hai.
5. Timer callback schedule hota hai.
6. Agla `console.log(number)` local `10` print karta hai.
7. Function finish hota hai.
8. Global `console.log(number)` `5` print karta hai.
9. Timer callback local lexical binding se `10` print karta hai.

**Important:** Local aur global variables ke naam same hain, lekin dono separate bindings hain.

---

# PART 8 — OUTPUT TRACING PRACTICE EXAMPLES

## 19. Example 1 — Synchronous Order

```js
console.log("A");
console.log("B");
console.log("C");
```

**Output:**

```text
A
B
C
```

## 20. Example 2 — Timer Order

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

console.log("C");
```

**Output:**

```text
A
C
B
```

## 21. Example 3 — Promise Order

```js
console.log("A");

Promise.resolve().then(() => {
  console.log("B");
});

console.log("C");
```

**Output:**

```text
A
C
B
```

## 22. Example 4 — Promise and Timer

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

**Output:**

```text
A
D
C
B
```

## 23. Example 5 — Nested Functions

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

`second()` complete hone ke baad `first()` continue hota hai.

---

# PART 9 — COMMON MISTAKES

## 24. Mistake: Zero-Millisecond Timer Immediately Runs

```js
setTimeout(() => {
  console.log("Timer");
}, 0);

console.log("Sync");
```

Correct output:

```text
Sync
Timer
```

Timer callback synchronous statement se pehle execute nahi hota.

## 25. Mistake: Promise Callback Timer Ke Baad Chalta Hai

```js
setTimeout(() => console.log("Timer"), 0);

Promise.resolve().then(() => console.log("Promise"));

console.log("Sync");
```

Correct output:

```text
Sync
Promise
Timer
```

Typical browser environment mein Promise microtask timer task se pehle process hoti hai.

## 26. Mistake: Har Callback Asynchronous Hota Hai

```js
function run(callback) {
  console.log("A");
  callback();
  console.log("C");
}

run(() => console.log("B"));
```

Correct output:

```text
A
B
C
```

Callback sirf ek function ko pass karne ka pattern hai. Callback synchronous bhi ho sakta hai.

## 27. Mistake: Timer Delay Exact Execution Time Hai

```js
setTimeout(() => {
  console.log("Finished");
}, 1000);
```

Iska matlab callback exactly 1 second par execute hone ki guarantee nahi. Synchronous work aur scheduling ki wajah se delay zyada ho sakta hai.

---

# PART 10 — FINAL REVISION

## 28. All Three Files: Quick Summary

| Concept                 | Key Point                                                               |
| ----------------------- | ----------------------------------------------------------------------- |
| JavaScript Engine       | Code ko process aur execute karta hai.                                  |
| Execution Context       | Code execution ka environment.                                          |
| Creation Phase          | Bindings prepare hoti hain; declarations ki type matter karti hai.      |
| Hoisting                | Declarations ko execution se pehle process karne ka behavior.           |
| TDZ                     | `let`/`const` ko initialization se pehle access nahi kar sakte.         |
| Call Stack              | Active function calls track karta hai.                                  |
| Primitive Assignment    | Value copy hoti hai.                                                    |
| Object Assignment       | Reference copy hota hai.                                                |
| Scope                   | Variable ki accessibility define karta hai.                             |
| Scope Chain             | Variable ko current aur outer environments mein search karta hai.       |
| Closure                 | Function outer lexical bindings ko retain karke access kar sakta hai.   |
| Synchronous Code        | Current execution mein order se run hota hai.                           |
| Asynchronous Operations | Completion aur callbacks baad mein handle ho sakte hain.                |
| Web APIs                | Browser environment ki additional facilities.                           |
| Task Queue              | Ready tasks execute hone ke liye wait kar sakte hain.                   |
| Event Loop              | Runtime scheduling process ka hissa.                                    |
| Promise Microtask       | Promise handlers aam tor par microtasks ke taur par schedule hote hain. |

## 29. Final Output-Tracing Checklist

Jab bhi JavaScript ka difficult output question solve karo, yeh order follow karo:

1. **Declarations:** `var`, `let`, `const` aur function declarations identify karo.
2. **Scope:** Har variable ki binding identify karo.
3. **Hoisting/TDZ:** Check karo ke variable us point par access ho sakta hai ya nahi.
4. **Function calls:** Call Stack aur nested function returns trace karo.
5. **Synchronous code:** Pehle current synchronous statements ka order nikaalo.
6. **Asynchronous operations:** Timer aur doosre callbacks schedule hone ko note karo.
7. **Microtasks:** Promise handlers ko synchronous code ke baad process karo.
8. **Timer tasks:** Ready timer callbacks ko uske baad consider karo.
9. **Closures:** Check karo ke callback kis lexical environment se variables access kar raha hai.
10. **Errors:** Agar uncaught error aaye, to dekho ke uske baad ka normal execution continue hota hai ya ruk jata hai.

## Final Summary

JavaScript Behind the Scenes ko samajhne ka sab se useful tareeqa yeh hai ke tum code ko sirf read na karo, balki execution order trace karo.

- Engine code execute karta hai.
- Execution Context variables aur execution environment ko manage karta hai.
- Call Stack function calls ka order track karta hai.
- Scope Chain variables ko outer environments mein resolve karti hai.
- Closures retained lexical bindings ko access karte hain.
- Asynchronous callbacks baad mein execute ho sakte hain.
- Typical browser output questions mein synchronous code ke baad Promise microtasks, phir ready timer tasks process hoti hain.

**Topic #6 — JavaScript Behind the Scenes: Complete.**
