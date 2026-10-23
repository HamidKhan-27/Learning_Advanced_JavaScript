# Advanced JavaScript #4 — Iterators & `for...of`

## 1. Iterator kya hota hai?

Iterator ka basic idea hai **data ko one-by-one access karna**.

JavaScript mein kuch data structures iterable hote hain, yani unke elements ko ek-ek karke access kiya ja sakta hai.

Common iterables:

- Array
- String
- Set
- Map

---

## 2. `for...of`

`for...of` kisi iterable ki **values ko one-by-one** access karta hai.

### Syntax

```js
for (let value of iterable) {
  // code
}
```

### Example

```js
let fruits = ["Apple", "Mango", "Banana"];

for (let fruit of fruits) {
  console.log(fruit);
}
```

Output:

```text
Apple
Mango
Banana
```

**Important:** `for...of` directly values deta hai, index nahi.

---

## 3. Normal `for` vs `for...of`

Normal `for`:

```js
let fruits = ["Apple", "Mango", "Banana"];

for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

`for...of`:

```js
for (let fruit of fruits) {
  console.log(fruit);
}
```

Jab sirf values chahiye hon, `for...of` zyada simple hota hai.

---

## 4. Array ke saath

```js
let languages = ["HTML", "CSS", "JavaScript"];

for (let language of languages) {
  console.log(language);
}
```

Output:

```text
HTML
CSS
JavaScript
```

Yahan `language` har iteration mein current value hai.

---

## 5. String ke saath

String bhi iterable hoti hai.

```js
let name = "Hamid";

for (let character of name) {
  console.log(character);
}
```

Output:

```text
H
a
m
i
d
```

---

## 6. `break`

`break` poore loop ko stop karta hai.

```js
let numbers = [10, 20, 30, 40];

for (let number of numbers) {
  if (number === 30) {
    break;
  }

  console.log(number);
}
```

Output:

```text
10
20
```

`30` par loop completely stop ho gaya.

---

## 7. `continue`

`continue` sirf current iteration ko skip karta hai.

```js
let numbers = [10, 20, 30, 40];

for (let number of numbers) {
  if (number === 30) {
    continue;
  }

  console.log(number);
}
```

Output:

```text
10
20
40
```

### Yaad rakho

```text
break     → poora loop stop
continue  → current iteration skip
```

---

# 8. Object aur `for...of`

Normal object directly `for...of` ke saath iterable nahi hota.

```js
let user = {
  name: "Hamid",
  city: "Karachi",
};
```

Isliye:

```js
for (let value of user) {
  console.log(value);
}
```

normal object ke liye kaam nahi karega.

Object ke saath `for...of` use karne ke liye `Object.keys()`, `Object.values()` ya `Object.entries()` use kar sakte hain.

---

## 9. `Object.values()`

`Object.values()` object ki **values** ko array mein deta hai.

```js
let user = {
  name: "Hamid",
  city: "Karachi",
};

console.log(Object.values(user));
```

Output:

```text
["Hamid", "Karachi"]
```

`for...of` ke saath:

```js
for (let value of Object.values(user)) {
  console.log(value);
}
```

Output:

```text
Hamid
Karachi
```

---

## 10. `Object.keys()`

`Object.keys()` object ki **keys** ko array mein deta hai.

```js
let user = {
  name: "Hamid",
  city: "Karachi",
};

for (let key of Object.keys(user)) {
  console.log(key);
}
```

Output:

```text
name
city
```

---

## 11. `Object.entries()`

`Object.entries()` **key + value** pairs deta hai.

```js
let user = {
  name: "Hamid",
  city: "Karachi",
};

console.log(Object.entries(user));
```

Conceptually:

```text
[
  ["name", "Hamid"],
  ["city", "Karachi"]
]
```

`for...of` ke saath:

```js
for (let [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```

Output:

```text
name Hamid
city Karachi
```

`[key, value]` yahan **destructuring** hai. Destructuring ko Topic #7 mein detail se parhenge.

---

# 12. `for...in` vs `for...of`

Ye difference bohot important hai.

### `for...in`

Keys/indexes deta hai.

```js
let user = {
  name: "Hamid",
  city: "Karachi",
};

for (let key in user) {
  console.log(key);
}
```

Output:

```text
name
city
```

Array mein:

```js
let fruits = ["Apple", "Mango"];

for (let index in fruits) {
  console.log(index);
}
```

Output:

```text
0
1
```

### `for...of`

Values deta hai.

```js
for (let fruit of fruits) {
  console.log(fruit);
}
```

Output:

```text
Apple
Mango
```

### Golden Rule

```text
for...in → keys / indexes
for...of → values
```

---

# 13. Set ke saath `for...of`

Set bhi iterable hota hai.

```js
let numbers = new Set([10, 20, 30]);

for (let number of numbers) {
  console.log(number);
}
```

Output:

```text
10
20
30
```

Set aur uske methods ko **Topic #8 — Set & Map** mein detail se parhenge.

---

# 14. Map ke saath `for...of`

Map bhi iterable hota hai.

```js
let user = new Map();

user.set("name", "Hamid");
user.set("city", "Karachi");

for (let item of user) {
  console.log(item);
}
```

Map entries pairs ki form mein milengi:

```text
["name", "Hamid"]
["city", "Karachi"]
```

Map ko bhi Topic #8 mein properly parhenge.

---

# 15. Practical Example

```js
let marks = [45, 80, 62, 30, 90];

for (let mark of marks) {
  if (mark >= 50) {
    console.log(`${mark} = Pass`);
  } else {
    console.log(`${mark} = Fail`);
  }
}
```

Output:

```text
45 = Fail
80 = Pass
62 = Pass
30 = Fail
90 = Pass
```

`for...of` ke andar conditions, template literals aur doosra normal JavaScript code use kar sakte hain.

---

# 16. Common Mistakes

### Mistake 1: Object ko directly iterate karna

```js
for (let value of user) {
  console.log(value);
}
```

Normal object ke liye ye work nahi karta.

Use:

```js
for (let value of Object.values(user)) {
  console.log(value);
}
```

### Mistake 2: `for...of` se index expect karna

```js
for (let fruit of fruits) {
  console.log(fruit);
}
```

Yahan `fruit` value hai:

```text
Apple
Mango
```

Index nahi.

### Mistake 3: `for...in` ko values samajhna

```js
for (let item in fruits) {
  console.log(item);
}
```

Yahan:

```text
0
1
```

milenge, values nahi.

---

# 17. Quick Revision

| Feature            | Kya deta hai?          |
| ------------------ | ---------------------- |
| `for...of`         | Values                 |
| `for...in`         | Keys / indexes         |
| `Object.keys()`    | Object ki keys         |
| `Object.values()`  | Object ki values       |
| `Object.entries()` | Key + value            |
| `break`            | Loop stop              |
| `continue`         | Current iteration skip |

### Common Iterables

```text
Array
String
Set
Map
```

### Most Important Formula

```text
for...in  → key / index
for...of  → value
```

### Object ke liye

```js
Object.keys(obj);
```

→ keys

```js
Object.values(obj);
```

→ values

```js
Object.entries(obj);
```

→ key + value

---

# 18. Final Example

```js
let students = ["Hamid", "Ali", "Ahmed", "Usman"];

for (let student of students) {
  if (student === "Ahmed") {
    continue;
  }

  console.log(student);
}
```

Output:

```text
Hamid
Ali
Usman
```

Yahan:

- Array iterable hai.
- `for...of` values de raha hai.
- `student` current value hai.
- `continue` Ahmed ko skip kar raha hai.

---

## Final Revision

**Iterator:** data ko one-by-one access karne ka concept.

**Iterable:** aisa data jise one-by-one iterate kiya ja sakta hai.

**`for...of`:** iterable ki values ko one-by-one access karta hai.

**`for...in`:** keys/indexes ko access karta hai.

**Array, String, Set aur Map** common iterables hain.

**Normal object** ko directly `for...of` se iterate nahi karte; `Object.keys()`, `Object.values()` ya `Object.entries()` use karte hain.

**Topic #4 — Iterators & `for...of` ✅ COMPLETE**
