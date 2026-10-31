# JavaScript — Topic #7

# Destructuring, Rest & Spread Operators

## Topics Covered

1. Array Destructuring
2. Skipping Array Elements
3. Default Values in Destructuring
4. Object Destructuring
5. Aliasing — Changing Variable Names
6. Rest Operator with Arrays
7. Rest Parameters in Functions
8. Object Rest Properties
9. Spread Operator with Arrays
10. Copying and Merging Arrays
11. Spread Operator with Objects
12. Rest vs Spread
13. Shallow Copy and Nested Objects
14. Common Mistakes and Revision

---

# PART 1 — ARRAY DESTRUCTURING

## 1. What Is Destructuring?

Destructuring is a JavaScript feature that allows us to extract values from arrays or properties from objects and store them in variables.

Without destructuring:

```javascript
const user = ["Hamid", 20, "Karachi"];

const name = user[0];
const age = user[1];
const city = user[2];

console.log(name); // Hamid
console.log(age); // 20
console.log(city); // Karachi
```

Using array destructuring:

```javascript
const user = ["Hamid", 20, "Karachi"];

const [name, age, city] = user;

console.log(name); // Hamid
console.log(age); // 20
console.log(city); // Karachi
```

### Explanation

- `user` is an array containing three values.
- Square brackets on the left create a destructuring pattern.
- `name` receives the first value.
- `age` receives the second value.
- `city` receives the third value.

**Important:** Array destructuring works according to position, not variable names.

```javascript
const numbers = [10, 20, 30];

const [a, b, c] = numbers;

console.log(a); // 10
console.log(b); // 20
console.log(c); // 30
```

The variable names could be anything. Their positions determine which values they receive.

---

## 2. Skipping Array Elements

We can skip an array element by leaving an empty space between commas.

```javascript
const colors = ["Red", "Blue", "Green", "Black"];

const [first, , third, fourth] = colors;

console.log(first); // Red
console.log(third); // Green
console.log(fourth); // Black
```

### Explanation

- `first` receives `"Red"`.
- The empty slot skips `"Blue"`.
- `third` receives `"Green"`.
- `fourth` receives `"Black"`.

**Remember:** Skipping an element does not remove it from the original array. It only prevents that value from being assigned to a variable in that position.

---

## 3. Default Values in Array Destructuring

A default value is used when the corresponding array value is `undefined`.

```javascript
const numbers = [10];

const [first, second = 20] = numbers;

console.log(first); // 10
console.log(second); // 20
```

The array has no second value, so `second` receives the default value `20`.

### Default value with undefined

```javascript
const [age = 18] = [undefined];

console.log(age); // 18
```

Because the supplied value is `undefined`, the default value is used.

### Default value with null

```javascript
const [age = 18] = [null];

console.log(age); // null
```

`null` is an actual supplied value, so the default is not used.

**Important rule:**

- `undefined` → default value is used.
- `null` → `null` remains the value.

---

# PART 2 — OBJECT DESTRUCTURING

## 4. What Is Object Destructuring?

Object destructuring extracts properties from an object and stores their values in variables.

Without destructuring:

```javascript
const user = {
  name: "Hamid",
  age: 20,
  city: "Karachi",
};

const name = user.name;
const age = user.age;
const city = user.city;

console.log(name); // Hamid
console.log(age); // 20
console.log(city); // Karachi
```

Using object destructuring:

```javascript
const user = {
  name: "Hamid",
  age: 20,
  city: "Karachi",
};

const { name, age, city } = user;

console.log(name); // Hamid
console.log(age); // 20
console.log(city); // Karachi
```

### Explanation

- Curly braces create the object destructuring pattern.
- `name` extracts the object's `name` property.
- `age` extracts the object's `age` property.
- `city` extracts the object's `city` property.

**Important difference:**

- Array destructuring depends on position.
- Object destructuring normally depends on property names.

Object property order does not determine which value is assigned.

```javascript
const user = {
  city: "Karachi",
  age: 20,
  name: "Hamid",
};

const { name, age } = user;

console.log(name); // Hamid
console.log(age); // 20
```

Even though `city` appears first in the object, `name` still receives `"Hamid"`.

---

## 5. Aliasing — Changing Variable Names

Sometimes we want to extract an object's property but use a different variable name.

We can do this using a colon (`:`).

```javascript
const user = {
  name: "Hamid",
  age: 20,
};

const { name: userName, age: userAge } = user;

console.log(userName); // Hamid
console.log(userAge); // 20
```

### Explanation

In this statement:

```javascript
const { name: userName } = user;
```

- `name` is the property name in the object.
- `userName` is the new variable name.
- The value `"Hamid"` is assigned to `userName`.

### Common mistake

```javascript
const user = {
  name: "Ali",
};

const { name: userName } = user;

console.log(userName); // Ali
console.log(name); // ReferenceError if name is undeclared
```

The destructuring statement creates `userName`, not a separate variable called `name`.

**Remember:** In object destructuring, `propertyName: variableName` means extract that property into the specified variable.

---

## 6. Default Values in Object Destructuring

We can provide a default value if a property is missing or its value is `undefined`.

```javascript
const user = {
  name: "Hamid",
};

const { name, age = 18, city = "Karachi" } = user;

console.log(name); // Hamid
console.log(age); // 18
console.log(city); // Karachi
```

The object has no `age` or `city` properties, so the defaults are used.

### When a property is null

```javascript
const user = {
  age: null,
};

const { age = 18 } = user;

console.log(age); // null
```

The default is not used because `age` contains `null`, not `undefined`.

---

# PART 3 — REST OPERATOR

## 7. Rest with Array Destructuring

The rest syntax (`...`) collects remaining values into a new array.

```javascript
const numbers = [10, 20, 30, 40, 50];

const [first, second, ...remaining] = numbers;

console.log(first); // 10
console.log(second); // 20
console.log(remaining); // [30, 40, 50]
```

### Explanation

1. `first` receives `10`.
2. `second` receives `20`.
3. `...remaining` collects all remaining values.
4. The collected values are stored in an array.

### Skipping and rest together

```javascript
const numbers = [10, 20, 30, 40, 50];

const [first, , third, ...rest] = numbers;

console.log(first); // 10
console.log(third); // 30
console.log(rest); // [40, 50]
```

The second array element, `20`, is skipped. The rest variable collects the elements after `30`.

**Important:** A rest element must be last in a destructuring pattern.

Correct:

```javascript
const [first, ...rest] = [10, 20, 30];
```

Incorrect:

```javascript
// SyntaxError: rest element must be last
const [...rest, last] = [10, 20, 30];
```

---

## 8. Rest Parameters in Functions

Rest parameters allow a function to accept any number of arguments and collect them into an array.

Without rest parameters, we might write:

```javascript
function add(a, b, c) {
  return a + b + c;
}

console.log(add(10, 20, 30)); // 60
```

This function has three named parameters.

Using rest parameters:

```javascript
function add(...numbers) {
  let total = 0;

  for (const number of numbers) {
    total += number;
  }

  return total;
}

console.log(add(10, 20)); // 30
console.log(add(10, 20, 30)); // 60
console.log(add(10, 20, 30, 40)); // 100
```

### Explanation

- `...numbers` collects all arguments into an array.
- The `for...of` loop visits each number.
- `total += number` adds each number to the running total.
- `return total` returns the final sum.

### Rest with a normal parameter

```javascript
function calculate(first, ...numbers) {
  console.log(first);
  console.log(numbers);
}

calculate(5, 10, 15, 20);
```

Output:

```javascript
(5)[(10, 15, 20)];
```

The first argument goes into `first`. All remaining arguments are collected into `numbers`.

**Important:** Rest parameters must be last.

Correct:

```javascript
function test(first, ...remaining) {
  console.log(first, remaining);
}
```

Incorrect:

```javascript
// SyntaxError
function test(...remaining, last) {}
```

---

## 9. Object Rest Properties

Object rest collects the remaining properties into a new object.

```javascript
const user = {
  name: "Hamid",
  age: 20,
  city: "Karachi",
  profession: "Developer",
};

const { name, ...details } = user;

console.log(name); // Hamid
console.log(details); // { age: 20, city: "Karachi", profession: "Developer" }
```

### Explanation

- `name` extracts the `name` property.
- `...details` collects all remaining enumerable own properties.
- The `name` property is not included in `details`.

This is useful when you want to separate one property from the rest of an object.

---

# PART 4 — SPREAD OPERATOR

## 10. What Is Spread?

Spread syntax (`...`) expands values from an iterable, such as an array, into individual values. In object literals, it copies enumerable own properties into a new object.

Compare these examples:

```javascript
const numbers = [10, 20, 30];

console.log(numbers); // [10, 20, 30]
console.log(...numbers); // 10 20 30
```

When spread is used inside `console.log`, the array's elements are passed as separate arguments.

**Rest collects values; spread expands values.**

---

## 11. Copying Arrays with Spread

We can create a shallow copy of an array using spread syntax.

```javascript
const original = [1, 2, 3];

const copy = [...original];

console.log(original); // [1, 2, 3]
console.log(copy); // [1, 2, 3]
```

The arrays contain the same values, but they are different array objects.

### Changing the copy

```javascript
const original = [1, 2, 3];

const copy = [...original];

copy[0] = 100;
copy.push(4);

console.log(original); // [1, 2, 3]
console.log(copy); // [100, 2, 3, 4]
```

### Explanation

- `copy[0] = 100` replaces the value at index `0`.
- `copy.push(4)` adds `4` to the end of the copy.
- The original array remains unchanged.

### Important: Replace vs Add

```javascript
const numbers = [1, 2, 3];

numbers[0] = 100; // Replaces the value at index 0
numbers.push(4); // Adds 4 at the end
numbers.unshift(0); // Adds 0 at the beginning

console.log(numbers); // [0, 100, 2, 3, 4]
```

Remember:

- `array[index] = value` → replaces the value at that index.
- `push()` → adds elements at the end.
- `unshift()` → adds elements at the beginning.

---

## 12. Merging Arrays with Spread

Spread can combine multiple arrays into one new array.

```javascript
const first = [1, 2, 3];
const second = [4, 5, 6];

const combined = [...first, ...second];

console.log(combined); // [1, 2, 3, 4, 5, 6]
```

We can also insert additional values while combining arrays.

```javascript
const first = ["Pen", "Book"];
const second = ["Laptop", "Phone"];

const items = ["Bag", ...first, "Bottle", ...second];

console.log(items);
// ["Bag", "Pen", "Book", "Bottle", "Laptop", "Phone"]
```

The values appear in the order in which they are written.

---

## 13. Spread with Objects

Object spread copies an object's enumerable own properties into a new object.

```javascript
const user = {
  name: "Hamid",
  age: 20,
};

const updatedUser = {
  ...user,
  city: "Karachi",
};

console.log(updatedUser);
// { name: "Hamid", age: 20, city: "Karachi" }

console.log(user);
// { name: "Hamid", age: 20 }
```

The original object remains unchanged in this example.

### Overriding properties

If the same property appears more than once, the last value wins.

```javascript
const user = {
  name: "Hamid",
  age: 20,
};

const updatedUser = {
  ...user,
  age: 25,
  city: "Lahore",
};

console.log(updatedUser);
// { name: "Hamid", age: 25, city: "Lahore" }

console.log(user);
// { name: "Hamid", age: 20 }
```

Here, `age: 25` appears after `...user`, so it overrides the copied `age: 20`.

### Property order matters for overriding

```javascript
const first = {
  name: "Ali",
  age: 20,
};

const second = {
  age: 30,
  ...first,
  city: "Karachi",
};

console.log(second);
// { age: 20, name: "Ali", city: "Karachi" }
```

The object initially receives `age: 30`, but `...first` then copies `age: 20` over it.

**Rule:** For duplicate property names, the last assigned value wins.

---

# PART 5 — REST VS SPREAD

## 14. Main Difference

| Rest                                                        | Spread                                                                          |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Collects remaining values                                   | Expands values                                                                  |
| Commonly used in destructuring                              | Commonly used in array/object literals and function calls                       |
| Used in function parameters to gather arguments             | Used in function calls to pass arguments separately                             |
| Produces an array in array destructuring or rest parameters | Depends on the context; it can expand iterable values or copy object properties |

Both use three dots (`...`). Their purpose depends on where they are used.

### Rest example

```javascript
function show(first, ...remaining) {
  console.log(first);
  console.log(remaining);
}

show(10, 20, 30, 40);

// Output:
// 10
// [20, 30, 40]
```

Rest collects the remaining arguments.

### Spread example

```javascript
const numbers = [10, 20, 30];

function show(a, b, c) {
  console.log(a, b, c);
}

show(...numbers);

// Output:
// 10 20 30
```

Spread expands the array's elements into separate function arguments.

### Using both together

```javascript
function test(a, ...rest) {
  return [a, ...rest];
}

const values = [10, 20, 30];

console.log(test(...values)); // [10, 20, 30]
```

Step by step:

1. `...values` spreads the array into arguments `10`, `20`, and `30`.
2. `a` receives `10`.
3. `...rest` collects `20` and `30` into `[20, 30]`.
4. `[a, ...rest]` creates a new array containing all three values.

---

# PART 6 — SHALLOW COPY

## 15. What Is a Shallow Copy?

Array and object spread create shallow copies, not deep copies.

For arrays containing only primitive values, changing a copied array's element does not change the corresponding element in the original array.

However, if an array or object contains another object, the nested object reference is copied rather than the nested object itself.

### Example with a nested object

```javascript
const original = {
  name: "Hamid",
  address: {
    city: "Karachi",
  },
};

const copy = { ...original };

copy.name = "Ali";
copy.address.city = "Lahore";

console.log(original.name); // Hamid
console.log(copy.name); // Ali
console.log(original.address.city); // Lahore
console.log(copy.address.city); // Lahore
```

### Why did this happen?

- `copy.name = "Ali"` changes a top-level property on the copied object.
- `copy.address.city = "Lahore"` changes the nested object.
- Both objects refer to the same nested `address` object.

Therefore, changing the nested city through `copy` also affects the city seen through `original`.

**Important:** Spread creates a shallow copy. It does not automatically clone nested objects.

---

# PART 7 — COMMON MISTAKES

## Mistake 1: Thinking array destructuring uses property names

```javascript
const numbers = [10, 20, 30];

const [third, first, second] = numbers;

console.log(third); // 10
console.log(first); // 20
console.log(second); // 30
```

Array destructuring uses positions. The variable named `third` still receives the first value.

## Mistake 2: Assuming null activates a default

```javascript
const [age = 18] = [null];

console.log(age); // null
```

Defaults apply to `undefined`, not `null`.

## Mistake 3: Placing rest before another element

```javascript
// Invalid syntax:
// const [...rest, last] = [1, 2, 3];
```

Rest must be last in the destructuring pattern or parameter list.

## Mistake 4: Confusing replacement with insertion

```javascript
const numbers = [1, 2, 3];

numbers[0] = 100;

console.log(numbers); // [100, 2, 3]
```

This replaces index `0`; it does not insert a new element.

## Mistake 5: Thinking spread always changes the original

```javascript
const original = [1, 2, 3];
const copy = [...original];

copy.push(4);

console.log(original); // [1, 2, 3]
console.log(copy); // [1, 2, 3, 4]
```

These are different arrays.

## Mistake 6: Forgetting that later object properties override earlier ones

```javascript
const first = { age: 20 };

const second = {
  ...first,
  age: 30,
};

console.log(second.age); // 30
```

The last assigned value for a property wins.

## Mistake 7: Assuming a shallow copy duplicates nested objects

```javascript
const original = {
  details: {
    city: "Karachi",
  },
};

const copy = { ...original };

copy.details.city = "Lahore";

console.log(original.details.city); // Lahore
```

The nested `details` object is shared.

---

# PART 8 — QUICK REVISION

## Array Destructuring

```javascript
const [a, b] = [10, 20];
```

- Uses positions.
- The first value goes to `a`.
- The second value goes to `b`.

## Object Destructuring

```javascript
const { name, age } = {
  name: "Ali",
  age: 20,
};
```

- Uses property names.
- Property order does not determine assignment.

## Default Values

```javascript
const [age = 18] = [undefined];
```

Output: `18`.

```javascript
const [age = 18] = [null];
```

Output: `null`.

## Rest

```javascript
const [first, ...rest] = [10, 20, 30];
```

`first` is `10`; `rest` is `[20, 30]`.

## Spread

```javascript
const copy = [...original];
```

Copies the array's elements into a new array.

## Object Spread

```javascript
const updated = {
  ...user,
  age: 25,
};
```

Copies the object's properties and sets `age` to `25`.

## Final Rules to Remember

1. Array destructuring is position-based.
2. Object destructuring is property-name-based.
3. Default values apply when the extracted value is `undefined`.
4. Rest collects remaining values and must be last.
5. Spread expands iterable values or copies enumerable object properties, depending on context.
6. `push()` adds at the end; `unshift()` adds at the beginning.
7. Assigning `array[index] = value` replaces the value at that index.
8. Later object properties override earlier properties with the same name.
9. Spread creates shallow copies, not automatic deep copies.
10. Rest and spread use the same `...` syntax but perform different jobs.

---

**Study strategy:** Read each section, type the examples yourself, predict the output before running the code, and then check your answer. Revisit the common mistakes before attempting a test without notes.
