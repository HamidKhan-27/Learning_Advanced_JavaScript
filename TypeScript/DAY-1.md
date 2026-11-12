# TypeScript – Fundamentals

## Topics Covered

1. Introduction to TypeScript
2. JavaScript vs TypeScript
3. Benefits of TypeScript
4. TypeScript Setup and Compilation
5. Basic Data Types
6. Array Types
7. Any and Unknown
8. Type Inference
9. Type Narrowing
10. Practical Examples

---

## 1. Introduction to TypeScript

TypeScript is a strongly typed programming language built on top of JavaScript. It adds a type system that helps developers identify type-related errors during development.

JavaScript is dynamically typed, which means a variable can hold different types of values.

**JavaScript Example:**

```javascript
let age = 20;

age = "Twenty";

console.log(age);
```

JavaScript allows this assignment.

**TypeScript Example:**

```typescript
let age: number = 20;

age = "Twenty"; // TypeScript Error
```

Here, TypeScript detects that a string is being assigned to a variable declared as a number.

**Important:** TypeScript adds static type checking to JavaScript. It does not replace JavaScript.

---

## 2. JavaScript vs TypeScript

| JavaScript                              | TypeScript                                           |
| --------------------------------------- | ---------------------------------------------------- |
| Dynamically typed                       | Supports static type checking                        |
| Uses `.js` extension                    | Uses `.ts` extension                                 |
| Types are checked during execution      | Types are checked during development and compilation |
| Browser executes JavaScript             | TypeScript is compiled into JavaScript               |
| Does not support TypeScript annotations | Supports type annotations                            |

### Why Use TypeScript?

- **Early error detection:** Identifies many type-related mistakes before execution.
- **Better code understanding:** Types explain what kind of values variables expect.
- **Autocomplete:** Editors can provide more accurate suggestions.
- **Maintainability:** Helps developers manage larger applications.
- **Development experience:** Makes complex code easier to understand.

TypeScript is commonly used with React, Node.js, APIs and large web applications.

---

## 3. TypeScript Setup and Compilation

TypeScript can be installed globally using npm.

```bash
npm install -g typescript
```

Check the installed version:

```bash
tsc --version
```

### Creating a TypeScript File

Create a file named `app.ts`.

```typescript
console.log("Hello TypeScript");
```

Compile the file:

```bash
tsc app.ts
```

This generates a JavaScript file named `app.js`.

Execute the generated file:

```bash
node app.js
```

**Output:**

```text
Hello TypeScript
```

### Compilation Process

```text
app.ts
   |
   | tsc app.ts
   v
app.js
   |
   | node app.js
   v
Program Output
```

**Important Concepts:**

- `.ts` is the TypeScript file extension.
- `tsc` is the TypeScript compiler command.
- TypeScript is converted into JavaScript.
- Browsers normally execute JavaScript, not TypeScript directly.

---

## 4. Basic Data Types

Type annotations specify the expected type of a variable.

General syntax:

```typescript
let variableName: type = value;
```

### A. String

The `string` type is used for text values.

```typescript
let studentName: string = "Hamid";

console.log(studentName);
```

**Output:**

```text
Hamid
```

A string variable accepts string values:

```typescript
let city: string = "Karachi";

city = "Lahore"; // Valid
city = 123; // Error
```

### B. Number

The `number` type is used for numeric values.

```typescript
let age: number = 18;
let marks: number = 90;
let price: number = 1500;

console.log(age);
console.log(marks);
console.log(price);
```

**Output:**

```text
18
90
1500
```

### C. Boolean

The `boolean` type stores only two values:

- `true`
- `false`

```typescript
let isStudent: boolean = true;
let isLoggedIn: boolean = false;

console.log(isStudent);
console.log(isLoggedIn);
```

**Output:**

```text
true
false
```

### Combining Basic Types

```typescript
let studentName: string = "Hamid";
let age: number = 18;
let isStudent: boolean = true;

console.log(studentName);
console.log(age);
console.log(isStudent);
```

**Output:**

```text
Hamid
18
true
```

---

## 5. Array Types

TypeScript allows us to define the type of elements stored inside an array.

### A. Number Array

```typescript
let marks: number[] = [85, 90, 75, 95];

console.log(marks);
```

**Output:**

```text
[85, 90, 75, 95]
```

`number[]` means the array should contain numbers.

### B. String Array

```typescript
let studentNames: string[] = ["Ali", "Ahmed", "Sara"];

console.log(studentNames);
```

**Output:**

```text
["Ali", "Ahmed", "Sara"]
```

### C. Generic Array Syntax

TypeScript also supports another syntax:

```typescript
let marks: Array<number> = [80, 90, 70];

let names: Array<string> = ["Ali", "Ahmed", "Sara"];
```

Both syntaxes are valid:

```typescript
let first: number[] = [10, 20, 30];

let second: Array<number> = [10, 20, 30];
```

**Remember:**

- `number[]` = array of numbers.
- `string[]` = array of strings.
- `Array<number>` = array of numbers.
- `Array<string>` = array of strings.

---

## 6. Any Type

The `any` type disables normal TypeScript type checking for a value.

```typescript
let data: any = "Hello";

data = 100;
data = true;

console.log(data);
```

All these assignments are allowed.

We can also call methods without TypeScript checking whether they are valid for the actual value.

```typescript
let value: any = "Hello";

console.log(value.toUpperCase());
```

**Output:**

```text
HELLO
```

### Why Should We Avoid Any?

Using `any` too often removes many of TypeScript's benefits because it allows operations without proper type checking.

Use it sparingly.

---

## 7. Unknown Type

The `unknown` type can hold values of different types, but it requires us to check the value's type before performing type-specific operations.

```typescript
let data: unknown = "Hello TypeScript";
```

The following code produces a TypeScript error:

```typescript
console.log(data.toUpperCase());
```

Why?

Because TypeScript does not know whether `data` is a string, number or another type.
We must check its type first.

```typescript
let data: unknown = "Hello TypeScript";

if (typeof data === "string") {
  console.log(data.toUpperCase());
}
```

**Output:**

```text
HELLO TYPESCRIPT
```

### Any vs Unknown

| Any                           | Unknown                                           |
| ----------------------------- | ------------------------------------------------- |
| Bypasses normal type checking | Requires checking before type-specific operations |
| Allows unchecked method calls | Does not allow unchecked method calls             |
| Can hide mistakes             | Helps prevent unsafe operations                   |
| Use sparingly                 | Prefer when the type is not yet known             |

---

## 8. Typeof Operator

The `typeof` operator checks the type of a value at runtime.

Example:

```typescript
let value = "Hello";

console.log(typeof value);
```

**Output:**

```text
string
```

Common results include:

| Value       | typeof result |
| ----------- | ------------- |
| `"Hello"`   | `"string"`    |
| `100`       | `"number"`    |
| `true`      | `"boolean"`   |
| `undefined` | `"undefined"` |

It is particularly useful when working with values typed as `unknown`.

---

## 9. Type Inference

Type inference means TypeScript automatically determines the type of a variable from its initial value.
We do not always need to write type annotations.

Example:

```typescript
let age = 20;
```

TypeScript automatically infers the type as `number`.

Similarly:

```typescript
let studentName = "Hamid";
let isStudent = true;
```

TypeScript infers:

- `studentName` → `string`
- `isStudent` → `boolean`

### Type Inference and Assignment

```typescript
let marks = 90;

marks = 95; // Valid
marks = "Ninety"; // Error
```

Even though we did not write `: number`, TypeScript still knows that `marks` is a number.

**Important:** Type inference reduces unnecessary type annotations while maintaining type checking.

---

## 10. Type Narrowing

Type narrowing means checking a variable's type so TypeScript can safely treat it as a more specific type.

It is commonly used with `unknown`.

Example:

```typescript
let userInput: unknown = "Hello TypeScript";

if (typeof userInput === "string") {
  console.log(userInput.toUpperCase());
} else {
  console.log("Not a string");
}
```

**Output:**

```text
HELLO TYPESCRIPT
```

Inside the `if` block, TypeScript knows that `userInput` is a string.

Therefore, it allows the use of `toUpperCase()`.

---

## 11. Practical Examples

### Example 1: Student Information

```typescript
let studentName: string = "Hamid";
let age: number = 18;
let isStudent: boolean = true;

console.log("Name:", studentName);
console.log("Age:", age);
console.log("Student:", isStudent);
```

**Output:**

```text
Name: Hamid
Age: 18
Student: true
```

### Example 2: Calculating the Sum of an Array

```typescript
let marks: number[] = [85, 90, 75, 95];

let totalMarks = 0;

for (let num of marks) {
  totalMarks += num;
}

console.log(totalMarks);
```

**Output:**

```text
345
```

**Explanation:**

1. `marks` stores an array of numbers.
2. `totalMarks` starts from zero.
3. The `for...of` loop takes each array element one by one.
4. `totalMarks += num` adds each number to the total.
5. TypeScript automatically infers `totalMarks` as a number.

### Example 3: Combining Arrays and Type Inference

```typescript
let marks: number[] = [85, 90, 75, 95];

let studentNames: string[] = ["Ali", "Ahmed", "Sara"];

let totalMarks = 0;

for (let num of marks) {
  totalMarks += num;
}

console.log(marks);
console.log(studentNames);
console.log(totalMarks);
```

**Output:**

```text
[85, 90, 75, 95]
["Ali", "Ahmed", "Sara"]
345
```

---

## 12. Important Points for Revision

1. TypeScript is built on JavaScript.
2. TypeScript adds static type checking.
3. TypeScript files use the `.ts` extension.
4. The `tsc` command compiles TypeScript into JavaScript.
5. Browsers normally execute JavaScript.
6. `string`, `number` and `boolean` are basic types.
7. `number[]` and `Array<number>` both represent number arrays.
8. `any` bypasses normal type checking.
9. `unknown` requires type checking before type-specific operations.
10. `typeof` checks a value's runtime type.
11. Type inference automatically determines types.
12. Type narrowing makes a broad type more specific after a check.
