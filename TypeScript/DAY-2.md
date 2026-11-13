# TypeScript – Day 2

## Union Types, Literal Types & Type Aliases

---

## Topics Covered

1. Union Types
2. Union Types with Variables and Arrays
3. Type Narrowing with `typeof`
4. Literal Types
5. String and Number Literal Types
6. Literal Types with Function Parameters
7. Type Aliases
8. Type Aliases with Union Types
9. Object Type Aliases
10. Optional Properties (`?`)
11. Readonly Properties
12. Type Aliases with Arrays
13. Nested Type Aliases
14. Practical Examples

---

# 1. Union Types

### What are Union Types?

Union Types allow a variable to accept values of multiple specified types.

The pipe symbol `|` is used to define a Union Type.

**Syntax:**

```ts
let variableName: string | number;
```

This means the variable can contain either a string or a number.

### Example

```ts
let userId: string | number;

userId = 101;
console.log(userId);

userId = "USER-101";
console.log(userId);
```

**Output:**

```text
101
USER-101
```

Both assignments are valid because the variable accepts `string` and `number`.

However:

```ts
userId = true; // TypeScript Error
```

Boolean is not included in the declared Union Type.

### Union Types with Multiple Types

We can specify more than two types.

```ts
let value: string | number | boolean;

value = "Hello";
value = 100;
value = true;
```

All three assignments are valid.

### Union Types with Arrays

We can also create arrays that accept multiple types.

```ts
let data: (string | number)[] = ["Hamid", 20, "Karachi", 500];
```

This array can contain strings and numbers.

But:

```ts
let data: (string | number)[] = ["Hamid", 20, true];
```

TypeScript reports an error because boolean is not allowed.

**Important Difference:**

```ts
let value: string | number;
```

A single variable that accepts either a string or a number.

```ts
let values: (string | number)[];
```

An array whose elements can be strings or numbers.

---

# 2. Type Narrowing with typeof

### What is Type Narrowing?

When a variable has multiple possible types, TypeScript sometimes cannot determine which type is currently being used.

Type Narrowing helps TypeScript identify the actual type before performing type-specific operations.

The `typeof` operator is commonly used for this purpose.

### Example

```ts
let value: string | number = "hello";

// TypeScript Error
console.log(value.toUpperCase());
```

The problem is that `toUpperCase()` is a string method, not a number method.

We can solve this using `typeof`.

```ts
let value: string | number = "hello";

if (typeof value === "string") {
  console.log(value.toUpperCase());
} else {
  console.log(value.toFixed(2));
}
```

**Output:**

```text
HELLO
```

### Another Example

```ts
function printValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.length);
  } else {
    console.log(value.toFixed(2));
  }
}

printValue("TypeScript");
printValue(25);
```

**Output:**

```text
10
25.00
```

Inside each branch, TypeScript knows which type is being used.

### Important typeof Checks

```ts
typeof "Hello"; // "string"
typeof 100; // "number"
typeof true; // "boolean"
```

---

# 3. Literal Types

### What are Literal Types?

Literal Types allow us to specify exact values that a variable can accept.

A normal type such as `string` allows any string value.

A Literal Type restricts the variable to specific predefined values.

### Normal String vs String Literal

Normal string:

```ts
let direction: string;

direction = "left";
direction = "right";
direction = "center";
direction = "anything";
```

All these values are valid because the type is `string`.

String Literal Union:

```ts
let direction: "left" | "right" | "up" | "down";

direction = "left";
direction = "right";
direction = "up";
direction = "down";

direction = "center"; // Error
```

Only the four specified values are allowed.

### Number Literal Types

Literal Types also work with numbers.

```ts
let dice: 1 | 2 | 3 | 4 | 5 | 6;

dice = 1;
dice = 4;
dice = 6;

dice = 7; // Error
```

Only numbers from 1 to 6 are allowed.

### Literal Types with Status Values

```ts
let orderStatus: "pending" | "shipped" | "delivered";

orderStatus = "pending";
orderStatus = "shipped";
orderStatus = "delivered";

orderStatus = "cancelled"; // Error
```

### Literal Types with Function Parameters

Literal Types can restrict the values passed to a function.

```ts
function setTheme(theme: "light" | "dark") {
  console.log("Selected theme:", theme);
}

setTheme("light");
setTheme("dark");

setTheme("blue"); // Error
```

The function only accepts `"light"` or `"dark"`.

### Literal Types with const and let

TypeScript treats `const` and `let` differently when inferring types.

```ts
const role = "admin";
```

TypeScript infers the literal type `"admin"` because a `const` variable cannot be reassigned.

But:

```ts
let role = "admin";
```

TypeScript normally infers the broader type `string`, because a `let` variable can be reassigned.

**Remember:**

- `const` can infer a literal type.
- `let` normally infers a broader type.
- Explicit Literal Types restrict the allowed values.

---

# 4. Type Aliases

### What is a Type Alias?

A Type Alias allows us to create a custom name for an existing type or a combination of types.

The `type` keyword is used to create Type Aliases.

**Syntax:**

```ts
type TypeName = ExistingType;
```

### Example with Union Types

```ts
type UserId = string | number;

let user1: UserId = 101;
let user2: UserId = "USER-102";

console.log(user1);
console.log(user2);
```

Instead of repeatedly writing `string | number`, we can simply use `UserId`.

### Type Aliases with Basic Types

```ts
type UserName = string;
type Age = number;
type IsActive = boolean;

let name: UserName = "Hamid";
let age: Age = 19;
let active: IsActive = true;
```

These aliases work like their original types.

### Type Aliases with Literal Types

```ts
type Status = "pending" | "approved" | "rejected";

let applicationStatus: Status;

applicationStatus = "pending";
applicationStatus = "approved";
applicationStatus = "rejected";

applicationStatus = "completed"; // Error
```

We can reuse the custom `Status` type wherever required.

---

# 5. Type Aliases with Objects

One of the most useful features of Type Aliases is defining object structures.

### Example

```ts
type Student = {
  name: string;
  age: number;
  email: string;
};

let student1: Student = {
  name: "Ali",
  age: 20,
  email: "ali@example.com",
};
```

Here, `Student` defines the structure that every Student object must follow.

### Reusing the Same Type

```ts
let student2: Student = {
  name: "Ahmed",
  age: 22,
  email: "ahmed@example.com",
};
```

We do not need to define the object structure again.

### TypeScript Object Validation

Missing required property:

```ts
let student3: Student = {
  name: "Hamza",
  age: 19,
};
```

TypeScript reports an error because `email` is missing.

Incorrect property type:

```ts
let student4: Student = {
  name: "Hamza",
  age: "19",
  email: "hamza@example.com",
};
```

This also produces an error because `age` must be a number.

---

# 6. Optional Properties (?)

Sometimes an object has properties that are not required.

Optional Properties allow us to create properties that may or may not be included when creating an object.

The `?` symbol is placed after the property name.

### Example

```ts
type Student = {
  name: string;
  age: number;
  email?: string;
};

let student1: Student = {
  name: "Ali",
  age: 20,
};

let student2: Student = {
  name: "Ahmed",
  age: 22,
  email: "ahmed@example.com",
};
```

Both objects are valid because `email` is optional.

### Required vs Optional

```ts
email: string;
```

The email property is required.

```ts
email?: string;
```

The email property is optional.

When an optional property is not provided, reading it can produce `undefined`.

Example:

```ts
console.log(student1.email);
```

**Output:**

```text
undefined
```

---

# 7. Readonly Properties

The `readonly` modifier prevents a property from being reassigned after the object has been created.

### Example

```ts
type User = {
  readonly id: number;
  name: string;
};

let user1: User = {
  id: 101,
  name: "Hamid",
};

user1.name = "Ali"; // Allowed

user1.id = 102; // Error
```

Here:

- `name` can be changed.
- `id` cannot be reassigned after initialization.

**Important:** `readonly` is a TypeScript compile-time restriction. It does not automatically make an object deeply immutable at JavaScript runtime.

---

# 8. Type Aliases with Arrays

We can create custom aliases for array types too.

### Number Array

```ts
type Marks = number[];

let studentMarks: Marks = [80, 90, 75, 88];

console.log(studentMarks);
```

Here, `Marks` represents `number[]`.

### String Array

```ts
type Subjects = string[];

let studentSubjects: Subjects = ["JavaScript", "TypeScript", "React"];
```

The `Subjects` alias can be reused wherever a string array is needed.

---

# 9. Type Aliases with Objects and Arrays

We can combine Object Type Aliases and Arrays.

First, define the Student structure:

```ts
type Student = {
  id: number;
  name: string;
  age: number;
  subjects: string[];
  email?: string;
};
```

Now create an array of Student objects:

```ts
let students: Student[] = [
  {
    id: 1,
    name: "Ali",
    age: 20,
    subjects: ["JavaScript", "TypeScript"],
  },

  {
    id: 2,
    name: "Ahmed",
    age: 22,
    subjects: ["HTML", "CSS"],
    email: "ahmed@example.com",
  },
];

console.log(students);
```

### Why Student[]?

`Student` represents the structure of one Student object.

`Student[]` represents an array containing multiple Student objects.

**Single Object:**

```ts
let student: Student = {
  id: 1,
  name: "Ali",
  age: 20,
  subjects: ["JavaScript"],
};
```

**Array of Objects:**

```ts
let students: Student[] = [
  {
    id: 1,
    name: "Ali",
    age: 20,
    subjects: ["JavaScript"],
  },
  {
    id: 2,
    name: "Ahmed",
    age: 22,
    subjects: ["HTML"],
  },
];
```

Every element inside the array must follow the `Student` structure.

---

# 10. Nested Type Aliases

A Type Alias can also be reused inside another Type Alias.

This is useful when working with complex objects.

### Example

```ts
type Address = {
  city: string;
  country: string;
};

type User = {
  name: string;
  age: number;
  address: Address;
};

let user1: User = {
  name: "Hamid",
  age: 19,

  address: {
    city: "Karachi",
    country: "Pakistan",
  },
};
```

Here:

- `Address` defines the address structure.
- `User` defines the user structure.
- `User` uses the `Address` alias as one of its properties.

This avoids repeating the same object structure.

---

# 11. Complete Practical Example

The following example combines several concepts from this lesson.

```ts
type OrderStatus = "pending" | "shipped" | "delivered";

type Customer = {
  readonly id: number;
  name: string;
  email?: string;
};

type Order = {
  orderId: number;
  status: OrderStatus;
  customer: Customer;
  total: number;
};

let orders: Order[] = [
  {
    orderId: 101,
    status: "pending",
    customer: {
      id: 1,
      name: "Ali",
      email: "ali@example.com",
    },
    total: 5000,
  },
  {
    orderId: 102,
    status: "shipped",
    customer: {
      id: 2,
      name: "Ahmed",
    },
    total: 8000,
  },
];

console.log(orders);
```

### Concepts Used

- `OrderStatus` is a Literal Union Type Alias.
- `Customer` defines a reusable object structure.
- `id` is readonly.
- `email` is optional.
- `Order` contains another Type Alias.
- `orders` is an array of Order objects.

This approach makes complex data structures easier to manage and maintain.

---

# 12. Important Revision Points

| Concept                 | Meaning                                          |
| ----------------------- | ------------------------------------------------ |
| `string \| number`      | Accepts either string or number                  |
| `(string \| number)[]`  | Array containing strings or numbers              |
| `typeof`                | Checks the runtime type of a value               |
| Type Narrowing          | Restricts a union to a specific type in a branch |
| Literal Type            | Allows only predefined values                    |
| `type`                  | Creates a custom type alias                      |
| `Student`               | Defines one Student object structure             |
| `Student[]`             | Array of Student objects                         |
| `email?`                | Optional property                                |
| `readonly`              | Prevents reassignment of a property              |
| `Address` inside `User` | Reuses a nested Type Alias                       |

## Final Notes

1. Use Union Types when a value can have more than one possible type.
2. Use Type Narrowing before calling methods that belong to a specific type.
3. Use Literal Types when only certain values should be accepted.
4. Use Type Aliases to create reusable custom types.
5. Use `?` for properties that are not required.
6. Use `readonly` when a property should not be reassigned.
7. Use `Student[]` when storing multiple Student objects.
8. Use nested Type Aliases to organize complex object structures.

**End of TypeScript Day 2 Study Material**
