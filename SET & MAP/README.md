# JavaScript Topic #8 — Set & Map

## Revision Notes

### Topics Covered

- Set and its methods
- Map and its methods
- Iteration and conversion
- Set vs Map vs Object
- Common mistakes and tricky questions

---

## 1. Set — Basic Concept

`Set` unique values store karta hai. Duplicate values automatically ignore ho jati hain.

```javascript
const numbers = new Set([1, 2, 2, 3, 3]);

console.log([...numbers]); // [1, 2, 3]
console.log(numbers.size); // 3
```

### Set Methods

| Method/Property | Purpose                       |
| --------------- | ----------------------------- |
| `new Set()`     | Empty Set banata hai          |
| `add(value)`    | Value add karta hai           |
| `has(value)`    | Value present hai ya nahi     |
| `delete(value)` | Ek value remove karta hai     |
| `clear()`       | Saari values remove karta hai |
| `size`          | Total unique values           |

```javascript
const fruits = new Set(["Apple", "Mango"]);

fruits.add("Banana");
console.log(fruits.has("Mango")); // true

fruits.delete("Apple");
console.log(fruits.size); // 2
```

**Important points:**

- Set mein `length` nahi, `size` use hota hai.
- `add()` duplicate value ko dobara insert nahi karta.
- `delete()` `true` ya `false` return karta hai.
- `clear()` saari values remove karta hai.

## 2. Set Order and Duplicate Rules

Set insertion order preserve karta hai.

```javascript
const set = new Set([1, 2, 3]);

set.delete(2);
set.add(2);

console.log([...set]); // [1, 3, 2]
```

- Duplicate primitive values remove hoti hain.
- `1` aur `"1"` different values hain.
- `NaN` ki multiple occurrences ek hi entry banti hain.
- Identical properties wale do separate objects duplicates nahi samjhe jate, kyunke unki references different hoti hain.

```javascript
const set = new Set([{ id: 1 }, { id: 1 }]);

console.log(set.size); // 2
```

## 3. Set Iteration and Array Conversion

```javascript
const colors = new Set(["Red", "Blue", "Green"]);

for (const color of colors) {
  console.log(color);
}
```

Array se duplicates remove karne ka short method:

```javascript
const numbers = [1, 2, 2, 3, 3, 4];

const unique = [...new Set(numbers)];

console.log(unique); // [1, 2, 3, 4]
```

Set ko Array mein convert karne ke liye `[...]` ya `Array.from(set)` use kar sakte ho.

---

## 4. Map — Basic Concept

`Map` key-value pairs store karta hai. Har key unique hoti hai.

```javascript
const marks = new Map();

marks.set("Ali", 80);
marks.set("Sara", 90);
marks.set("Ali", 95);

console.log(marks.get("Ali")); // 95
console.log(marks.size); // 2
```

`"Ali"` ki value update hui; duplicate key create nahi hui.

### Map Methods

| Method/Property   | Purpose                     |
| ----------------- | --------------------------- |
| `new Map()`       | Empty Map                   |
| `set(key, value)` | Entry add/update            |
| `get(key)`        | Key ki value hasil karna    |
| `has(key)`        | Key check karna             |
| `delete(key)`     | Ek entry remove karna       |
| `clear()`         | Saari entries remove karna  |
| `size`            | Total keys/entries          |
| `keys()`          | Keys ka iterator            |
| `values()`        | Values ka iterator          |
| `entries()`       | Key-value pairs ka iterator |

**Important:** Missing key ke liye `get()` `undefined` return karta hai, jabke `has()` `false` return karta hai.

## 5. Map Keys and Insertion Order

Map mein strings, numbers, Booleans aur objects keys ban sakte hain.

```javascript
const map = new Map();

map.set(1, "Number");
map.set("1", "String");

console.log(map.size); // 2
```

Object keys reference ke zariye compare hoti hain:

```javascript
const a = { id: 1 };
const b = { id: 1 };

const map = new Map();
map.set(a, "First");

console.log(map.get(a)); // "First"
console.log(map.get(b)); // undefined
```

### Updating vs Re-Adding

```javascript
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3],
]);

map.set("b", 20);
console.log([...map.keys()]); // ["a", "b", "c"]

map.delete("b");
map.set("b", 20);
console.log([...map.keys()]); // ["a", "c", "b"]
```

- Existing key update karne se order change nahi hota.
- Delete karke dobara add karne se key end par chali jati hai.

## 6. Looping Through Map

```javascript
const prices = new Map([
  ["Apple", 100],
  ["Mango", 150],
]);

for (const [fruit, price] of prices) {
  console.log(fruit, price);
}

// Apple 100
// Mango 150
```

- `for...of` Map se `[key, value]` pairs deta hai.
- `keys()` sirf keys deta hai.
- `values()` sirf values deta hai.
- `entries()` key-value pairs deta hai.

**Important difference:** Map ka `forEach()` callback arguments ki order `(value, key, map)` hoti hai. `for...of` mein destructuring order `[key, value]` hota hai.

---

## 7. Set vs Map vs Object

| Feature           | Set           | Map               | Object                      |
| ----------------- | ------------- | ----------------- | --------------------------- |
| Stores            | Unique values | Key-value pairs   | Properties                  |
| Add/update        | `add(value)`  | `set(key, value)` | `obj.key = value`           |
| Retrieve          | No `get()`    | `get(key)`        | `obj.key`                   |
| Check             | `has(value)`  | `has(key)`        | `Object.hasOwn()`           |
| Count             | `size`        | `size`            | `Object.keys(obj).length`\* |
| Direct `for...of` | Yes           | Yes               | No                          |

\* `Object.keys(obj).length` enumerable own string-keyed properties count karta hai.

**Kab use karein?**

- **Set:** unique values chahiye hon.
- **Map:** flexible keys aur key-value entries chahiye hon.
- **Object:** kisi entity ka structured data, jaise product ka `name`, `price`, `brand`.

## 8. Common Mistakes

```javascript
const set = new Set([1, 2, 3]);

// set.push(4); // Wrong: Set mein push() nahi
set.add(4); // Correct

console.log(set.size); // 4
```

```javascript
const map = new Map();

// map.add("Ali", 90); // Wrong: Map mein add() nahi
map.set("Ali", 90); // Correct

console.log(map.get("Ali")); // 90
```

Yaad rakho:

- Set mein `add()`, Map mein `set()`.
- Set mein `get()` nahi hota.
- Set aur Map dono mein `size` hota hai, `length` nahi.
- Set duplicate values rokta hai; Map existing key ki value update karta hai.

## 9. Hard-Test Corrections

### Q7 — Map Key Update

```javascript
const map = new Map([
  ["x", 1],
  ["y", 2],
  ["z", 3],
]);

map.set("y", 99);

console.log([...map.keys()]);
// ["x", "y", "z"]
```

**Reason:** Existing key update hone par insertion order same rehta hai.

### Q8 — Set Value Delete

```javascript
const set = new Set([1, 2, 3]);

for (const value of set) {
  if (value === 3) {
    set.delete(3);
  }
}

console.log([...set]);
// [1, 2]
```

**Reason:** `3` delete ho gaya aur dobara add nahi hua, is liye final Set mein sirf `1` aur `2` hain.

---

## Final Revision Checklist

- [ ] Set duplicates remove karta hai.
- [ ] Set aur Map mein `size` use hota hai.
- [ ] Set ke methods: `add`, `has`, `delete`, `clear`.
- [ ] Map ke methods: `set`, `get`, `has`, `delete`, `clear`.
- [ ] `[...new Set(array)]` Array se duplicate primitive values remove karta hai.
- [ ] Map iteration mein `[key, value]` milta hai.
- [ ] Map `forEach()` mein `(value, key, map)` milta hai.
- [ ] Objects ki identical properties ka matlab identical reference nahi.
- [ ] Map key update order nahi badalta; delete + re-add order badal deta hai.

**Topic #8 — Set & Map: Completed.**

**Next topic: #9 — Default Parameters.**
