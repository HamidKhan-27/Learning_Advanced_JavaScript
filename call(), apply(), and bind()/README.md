# JavaScript Advanced Study Notes

## Topic #12 — call(), apply(), and bind()

### Topics Covered

1. Understanding `this`
2. `call()` method
3. `apply()` method
4. `bind()` method
5. Difference between all three methods
6. Arguments and return values
7. Bound functions and `this`
8. Class methods and event listeners
9. Arrow function limitation
10. Common mistakes and revision

---

## 1. Understanding `this`

`this` ek special JavaScript keyword hai. Normal functions mein iska value aam tor par is baat par depend karta hai ke function ko kaise call kiya gaya.

```js
const user = {
  name: "Hamid",

  greet: function () {
    console.log(this.name);
  },
};

user.greet(); // Hamid
```

**Explanation:**

- `user.greet()` mein `this` `user` object ko refer karta hai.
- `this.name` ka matlab `user.name` hai.
- Normal function ko alag tareeqe se call karne par `this` ka behaviour badal sakta hai.

## 2. call() Method

`call()` function ko foran execute karta hai aur normal function ke liye `this` set kar sakta hai.

### Syntax

```js
functionName.call(thisValue, arg1, arg2);
```

### Example

```js
function introduce(city, country) {
  console.log(this.name + " lives in " + city + ", " + country);
}

const user = {
  name: "Hamid",
};

introduce.call(user, "Karachi", "Pakistan");
```

Output:

```text
Hamid lives in Karachi, Pakistan
```

**Yaad rakho:**

- Pehla argument `this` ke liye hota hai.
- Baqi arguments alag-alag pass hote hain.
- `call()` function ko foran execute karta hai.

## 3. apply() Method

`apply()` bhi function ko foran execute karta hai aur `this` set kar sakta hai.

Iska main farq arguments pass karne ka tareeqa hai.

### Syntax

```js
functionName.apply(thisValue, [arg1, arg2]);
```

### Example

```js
function introduce(city, country) {
  console.log(this.name + " lives in " + city + ", " + country);
}

const user = {
  name: "Hamid",
};

introduce.apply(user, ["Karachi", "Pakistan"]);
```

Output:

```text
Hamid lives in Karachi, Pakistan
```

**Important:** `apply()` mein arguments array ke andar diye jate hain.

## 4. bind() Method

`bind()` ek naya function return karta hai jismein `this` pehle se set hota hai. Yeh original function ko foran execute nahi karta.

### Syntax

```js
const newFunction = functionName.bind(thisValue);
```

### Example

```js
const user = {
  name: "Hamid",
};

function greet() {
  console.log("Hello " + this.name);
}

const greetUser = greet.bind(user);

greetUser(); // Hello Hamid
greetUser(); // Hello Hamid
```

**Explanation:**

- `greet.bind(user)` ek naya function return karta hai.
- `bind()` wali line par `greet` execute nahi hota.
- `greetUser()` call karne par function execute hota hai.
- Bound function ka `this` `user` hi rehta hai.

## 5. call() vs apply() vs bind()

| Feature                | call()                    | apply()                   | bind()             |
| ---------------------- | ------------------------- | ------------------------- | ------------------ |
| Function foran execute | Haan                      | Haan                      | Nahi               |
| `this` set karna       | Haan                      | Haan                      | Haan               |
| Arguments              | Alag-alag                 | Array mein                | Pehle ya baad mein |
| Result                 | Function ka return result | Function ka return result | Naya function      |

### Same function, three methods

```js
const user = {
  name: "Hamid",
};

function greet(greeting) {
  return greeting + ", " + this.name;
}

console.log(greet.call(user, "Hello"));
// Hello, Hamid

console.log(greet.apply(user, ["Hi"]));
// Hi, Hamid

const welcome = greet.bind(user, "Welcome");

console.log(welcome());
// Welcome, Hamid
```

## 6. Arguments with bind()

`bind()` mein arguments pehle se set kar sakte hain.

```js
function introduce(city, country) {
  console.log(this.name + " lives in " + city + ", " + country);
}

const user = {
  name: "Hamid",
};

const introduceHamid = introduce.bind(user, "Karachi");

introduceHamid("Pakistan");
// Hamid lives in Karachi, Pakistan
```

Yahan `"Karachi"` pehle set ho gaya. `"Pakistan"` baad mein function call karte waqt diya.

## 7. Important: bind() Binding Does Not Change

Yeh Topic #12 ka bohat important point hai.

```js
const user = {
  name: "Ali",
};

function greet() {
  return this.name;
}

const fn = greet.bind(user);

console.log(fn.call({ name: "Sara" }));
// Ali
```

**Ali kyun print hota hai?**

1. `greet.bind(user)` ne `this` ko `user` ke saath bind kiya.
2. `fn.call({ name: "Sara" })` baad mein call hua.
3. Lekin bound function ka `this` change nahi hua.
4. Is liye result `"Ali"` hai, `"Sara"` nahi.

Yahi rule `fn.apply(...)` par bhi apply hota hai.

## 8. Class Methods and bind()

Class objects banane ka blueprint hoti hai. Class ke andar defined functions ko methods kaha jata hai.

```js
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello " + this.name);
  }
}

const user1 = new User("Hamid");

user1.greet(); // Hello Hamid
```

Agar method ka reference alag variable mein rakh dein, to `this` ka context lose ho sakta hai.

```js
const greetUser = user1.greet.bind(user1);

greetUser(); // Hello Hamid
```

`bind(user1)` method ka `this` `user1` ke saath set kar deta hai.

## 9. Event Listeners and bind()

```js
const user = {
  name: "Hamid",

  greet: function () {
    console.log("Hello " + this.name);
  },
};

const greetUser = user.greet.bind(user);

// HTML page mein button maujood hona chahiye.
document.querySelector("button").addEventListener("click", greetUser);
```

Button click hone par `greetUser` execute hoga. `bind(user)` ki wajah se `this.name` `"Hamid"` rahega.

## 10. Arrow Functions and this

Arrow functions apna alag `this` create nahi kartin. Yeh surrounding lexical scope se `this` leti hain.

Is liye arrow function ke `this` ko `call()`, `apply()` ya `bind()` se normal function ki tarah change nahi kiya ja sakta.

```js
const user = {
  name: "Hamid",

  greet: () => {
    console.log(this.name);
  },
};

user.greet();
```

Is example mein `this` `user` ko refer karna guaranteed nahi hai. Object method ke liye aam tor par normal method syntax use karo:

```js
const user = {
  name: "Hamid",

  greet() {
    console.log(this.name);
  },
};

user.greet(); // Hamid
```

## 11. Common Mistakes

1. `call()` aur `apply()` ko `bind()` jaisa samajhna.
2. `call()` mein arguments array ki form mein pass karna jab zaroorat na ho.
3. `apply()` mein multiple arguments alag-alag pass karna.
4. Yeh samajhna ke `bind()` original function ko foran execute karta hai.
5. Yeh samajhna ke bound function par `call()` se uska `this` badal jayega.
6. Arrow function ka `this` normal function ki tarah set karne ki koshish karna.

## 12. Quick Revision

- `call()` — foran execute; arguments alag-alag.
- `apply()` — foran execute; arguments array mein.
- `bind()` — naya function return; execution baad mein.
- `this` ka behaviour function type aur calling context par depend karta hai.
- Bound function ka `this` baad ke `call()` ya `apply()` se replace nahi hota.
- Arrow functions surrounding scope se `this` leti hain.
- `bind()` event listeners aur methods ke context ko preserve karne mein useful hai.

### Final Memory Trick

**Call = Comma-separated arguments**

**Apply = Array of arguments**

**Bind = Build a new bound function**
