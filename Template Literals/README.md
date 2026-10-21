# Advanced JavaScript #2 — Template Literals

## 1. Template Literals kya hain?

JavaScript mein strings normally single quotes `' '` ya double quotes `" "` se banate hain:

```js
let name = "Hamid";
let city = 'Karachi';
```

Modern JavaScript mein strings banane ka ek aur useful tareeqa **Template Literals** hai.

Template Literals mein **backticks** use hote hain:

```js
let name = `Hamid`;
```

Backtick:

```text
`
```

> **Important:** Backtick (`) aur single quote (`'`) different hain.

---

## 2. Basic Syntax

```js
let message = `Hello World`;

console.log(message);
```

Output:

```text
Hello World
```

Normal string:

```js
let message = "Hello World";
```

Template Literal:

```js
let message = `Hello World`;
```

Dono strings hain, lekin Template Literals mein additional features milte hain.

---

# 3. Variable ko String ke Andar Use Karna

Template Literals ka sabse important feature **interpolation** hai.

Syntax:

```js
${variable}
```

Example:

```js
let name = "Hamid";

console.log(`Hello ${name}`);
```

Output:

```text
Hello Hamid
```

JavaScript `${name}` ki jagah `name` ki actual value insert karta hai.

### Formula

```js
`Normal text ${variable} normal text`
```

---

# 4. Multiple Variables

Ek Template Literal mein multiple variables use kar sakte hain:

```js
let name = "Hamid";
let age = 20;
let city = "Karachi";

console.log(
    `My name is ${name}, I am ${age} years old, and I live in ${city}.`
);
```

Output:

```text
My name is Hamid, I am 20 years old, and I live in Karachi.
```

Traditional concatenation mein:

```js
let message =
    "My name is " + name +
    ", I am " + age +
    " years old, and I live in " + city + ".";
```

Template Literal mein code zyada readable hai:

```js
let message = `My name is ${name}, I am ${age} years old, and I live in ${city}.`;
```

---

# 5. Expressions inside `${}`

`${}` ke andar sirf variable nahi, **JavaScript expression** bhi likh sakte hain.

Example:

```js
let a = 10;
let b = 20;

console.log(`Total: ${a + b}`);
```

Output:

```text
Total: 30
```

JavaScript pehle:

```js
a + b
```

calculate karega aur phir result string mein insert karega.

---

## 6. Mathematical Expressions

```js
let price = 500;
let quantity = 3;

console.log(`Total price is ${price * quantity}`);
```

Output:

```text
Total price is 1500
```

Another example:

```js
let age = 20;

console.log(`Next year I will be ${age + 1}`);
```

Output:

```text
Next year I will be 21
```

Aap ye operators bhi use kar sakte hain:

```js
`${a + b}`
`${a - b}`
`${a * b}`
`${a / b}`
```

---

# 7. Function Call inside `${}`

Function ko bhi `${}` ke andar call kar sakte hain.

```js
function greet() {
    return "Hello";
}

console.log(`${greet()} Hamid`);
```

Output:

```text
Hello Hamid
```

Function ka returned result Template Literal mein insert ho jata hai.

### Function with arguments

```js
function add(a, b) {
    return a + b;
}

console.log(`Answer is ${add(10, 20)}`);
```

Output:

```text
Answer is 30
```

Yahan:

```js
${add(10, 20)}
```

function call hai.

---

# 8. Multiple Lines

Template Literals ka ek important advantage **multiline strings** hain.

Normal string:

```js
let message = "Hello\nMy name is Hamid\nI live in Karachi";
```

Template Literal:

```js
let message = `Hello
My name is Hamid
I live in Karachi`;

console.log(message);
```

Output:

```text
Hello
My name is Hamid
I live in Karachi
```

Backticks ke andar line breaks directly preserve ho jate hain.

---

# 9. Multiple Lines + Variables

```js
let name = "Hamid";
let city = "Karachi";

let message = `
Hello ${name}
You live in ${city}
Welcome to JavaScript
`;

console.log(message);
```

Output:

```text
Hello Hamid
You live in Karachi
Welcome to JavaScript
```

---

# 10. Object Properties

Object ki properties bhi `${}` ke andar use kar sakte hain.

```js
let user = {
    name: "Hamid",
    city: "Karachi"
};

console.log(`My name is ${user.name}`);
console.log(`I live in ${user.city}`);
```

Output:

```text
My name is Hamid
I live in Karachi
```

Yahan:

```js
${user.name}
```

object ki `name` property access kar raha hai.

---

# 11. Array Values

Array ki values bhi use kar sakte hain:

```js
let fruits = ["Apple", "Mango", "Banana"];

console.log(`My favorite fruit is ${fruits[1]}`);
```

Output:

```text
My favorite fruit is Mango
```

Kyunki:

```js
fruits[1]
```

ki value `"Mango"` hai.

---

# 12. Condition ke Saath Template Literal

`${}` ke andar expression hone ki wajah se ternary operator bhi use kar sakte hain.

```js
let age = 20;

console.log(
    `I am ${age >= 18 ? "Adult" : "Minor"}`
);
```

Output:

```text
I am Adult
```

Agar `age` 18 se kam hoti to:

```text
I am Minor
```

---

# 13. HTML ke Saath Template Literals

Web development mein Template Literals ka practical use bohot important hai.

Example:

```js
let name = "Hamid";

let html = `
    <div>
        <h2>${name}</h2>
        <p>Welcome to my website.</p>
    </div>
`;

console.log(html);
```

Output mein dynamically:

```html
<div>
    <h2>Hamid</h2>
    <p>Welcome to my website.</p>
</div>
```

Yahan `${name}` ki value HTML ke andar insert hui.

---

# 14. Dynamic Product Example

```js
let product = "Laptop";
let price = 80000;
let quantity = 2;

let html = `
    <div>
        <h2>${product}</h2>
        <p>Price: ${price}</p>
        <p>Quantity: ${quantity}</p>
        <p>Total: ${price * quantity}</p>
    </div>
`;

console.log(html);
```

Output:

```html
<div>
    <h2>Laptop</h2>
    <p>Price: 80000</p>
    <p>Quantity: 2</p>
    <p>Total: 160000</p>
</div>
```

Is tarah Template Literals future mein dynamic UI aur DOM work mein useful honge.

---

# 15. Common Mistake — Double Quotes

Ye code:

```js
let name = "Hamid";

console.log("Hello ${name}");
```

Output:

```text
Hello ${name}
```

Variable ki value insert nahi hogi.

### Correct:

```js
console.log(`Hello ${name}`);
```

Output:

```text
Hello Hamid
```

**Reason:** Interpolation ke liye backticks zaroori hain.


# 18. Template Literals vs Concatenation

### Concatenation

```js
let name = "Hamid";
let age = 20;

let message =
    "My name is " + name + " and I am " + age + " years old.";
```

### Template Literal

```js
let name = "Hamid";
let age = 20;

let message =
    `My name is ${name} and I am ${age} years old.`;
```

Template Literal mein dynamic values ko read aur manage karna generally easier hota hai.

---

# 19. Practical Combined Example

```js
let user = {
    name: "Hamid",
    age: 20,
    city: "Karachi"
};

let message = `
Name: ${user.name}
Age: ${user.age}
City: ${user.city}
Next year age: ${user.age + 1}
`;

console.log(message);
```

Output:

```text
Name: Hamid
Age: 20
City: Karachi
Next year age: 21
```

Is example mein:

* Object property access
* Variable interpolation
* Mathematical expression
* Multiple lines

sab use hua.

---

# 20. Template Literals ke Main Features

| Feature        | Example                             |
| -------------- | ----------------------------------- |
| Backticks      | `` `Hello` ``                       |
| Variable       | `` `${name}` ``                     |
| Expression     | `` `${a + b}` ``                    |
| Function       | `` `${add(2, 3)}` ``                |
| Object         | `` `${user.name}` ``                |
| Array          | `` `${fruits[0]}` ``                |
| Condition      | `` `${age >= 18 ? "Yes" : "No"}` `` |
| Multiple lines | Backticks ke andar line breaks      |

---

# 21. Important Rules

> **Rule 1:** Template Literals ke liye backticks `` ` `` use hote hain.
> **Rule 2:** Variable ya expression insert karne ke liye `${}` use hota hai.
> **Rule 3:** `${}` ke andar JavaScript expression likh sakte hain.
> **Rule 4:** Template Literals multiple lines support karte hain.
> **Rule 5:** Single aur double quotes ke andar `${}` interpolation nahi karta.
> **Rule 6:** Template Literals dynamic strings aur HTML banane mein useful hain.

---

# 22. Quick Revision

### Basic

```js
let message = `Hello World`;
```

### Variable

```js
let name = "Hamid";

console.log(`Hello ${name}`);
```

### Multiple Variables

```js
let name = "Hamid";
let city = "Karachi";

console.log(`My name is ${name} and I live in ${city}.`);
```

### Expression

```js
let price = 500;
let quantity = 3;

console.log(`Total: ${price * quantity}`);
```

### Function

```js
function add(a, b) {
    return a + b;
}

console.log(`Answer: ${add(10, 20)}`);
```

### Multiple Lines

```js
let message = `
Hello Hamid
Welcome to JavaScript
Keep Learning
`;
```

### Object

```js
let user = {
    name: "Hamid"
};

console.log(`Hello ${user.name}`);
```

---

# 23. Master Formula

Template Literals ka main syntax:

```js
`Normal text ${JavaScript expression} normal text`
```

Example:

```js
let name = "Hamid";
let age = 20;
let city = "Karachi";

let message = `
Name: ${name}
Age: ${age}
City: ${city}
Next year: ${age + 1}
`;

console.log(message);
```

### Final Concept

```text
Backticks
    ↓
Template Literal
    ↓
${}
    ↓
Variable / Expression
    ↓
Result string mein insert
```
**#2 Template Literals — Complete.**
