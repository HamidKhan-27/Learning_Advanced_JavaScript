# TypeScript – Day 3

## Interfaces, Type vs Interface & Function Types

## Topics Covered

1. Interfaces
2. Optional and Readonly Properties
3. Nested Interfaces and Interface Arrays
4. Type Alias vs Interface
5. Extending Interfaces
6. Intersection Types
7. Declaration Merging
8. Function Parameters and Return Types
9. Void Return Type
10. Optional and Default Parameters
11. Function Type Aliases
12. Object Types in Functions
13. Nullish Coalescing (`??`)
14. Master Complete Code Example

---

# Part 1 – Interfaces

## 1. What is an Interface?

An **Interface** is a TypeScript feature used to define the structure of an object.

It specifies which properties an object must contain and what types those properties should have.

When we create an object using an interface, TypeScript checks whether the object follows the defined structure.

This helps us:

- Maintain consistent object structures.
- Catch incorrect property types.
- Avoid missing required properties.
- Reuse the same structure in multiple objects and functions.

### Example

```ts
interface Student {
  name: string;
  age: number;
  isEnrolled: boolean;
}

const student1: Student = {
  name: "Ali",
  age: 20,
  isEnrolled: true,
};
```

**Explanation:**

- `interface Student` defines a structure named Student.
- `name` must be a string.
- `age` must be a number.
- `isEnrolled` must be a boolean.
- `student1: Student` tells TypeScript to check the object against this interface.

If a required property is missing or its type is incorrect, TypeScript reports an error.

An interface does not create an object. It only describes the structure that an object should follow.

## 2. Optional Properties (`?`)

By default, all properties in an interface are required.

Sometimes an object may have a property that is not necessary for every object. In that situation, we use `?` after the property name.

```ts
interface User {
  name: string;
  email?: string;
}
```

Here:

- `name` is required.
- `email` is optional.

An object can follow this interface with or without an email property.

**Important:** An optional property can be `undefined` when it is not provided. You should handle that possibility when using its value.

## 3. Readonly Properties

The `readonly` keyword prevents a property from being reassigned after the object has been created.

```ts
interface Employee {
  readonly id: number;
  name: string;
}
```

The `id` property can be given a value when creating an object, but it cannot be reassigned later.

**Important distinction:**

`readonly` provides a TypeScript compile-time restriction. It does not automatically freeze the complete object at runtime.

For example, if a readonly property contains an array, `readonly` prevents assigning a different array to that property. It does not automatically prevent changing the contents of the existing array.

## 4. Nested Interfaces

An interface can contain another interface as one of its properties.

This is useful when an object has a more complex structure.

For example, a student may have personal information and an address. Instead of defining every property in one large interface, we can separate the structures.

```ts
interface Address {
  city: string;
  country: string;
}

interface Student {
  name: string;
  address: Address;
}
```

The `address` property must follow the structure defined by the `Address` interface.

**Benefits:**

- Keeps complex structures organized.
- Makes interfaces reusable.
- Improves readability.
- Makes nested object types easier to maintain.

## 5. Interface Arrays

An interface can also define the structure of objects stored inside an array.

For example:

```ts
interface Product {
  name: string;
  price: number;
}

const products: Product[] = [
  { name: "Laptop", price: 80000 },
  { name: "Mobile", price: 40000 },
];
```

`Product[]` means an array containing Product objects.

Every object in the array must follow the Product interface.

The same concept can also be written using generic array syntax:

`Array<Product>`

Both forms represent an array of Product objects.

---

# Part 2 – Type Alias vs Interface

## 6. What is a Type Alias?

A Type Alias allows us to create a custom name for a type using the `type` keyword.

It can represent:

- Primitive types.
- Union types.
- Literal types.
- Object structures.
- Arrays.
- Function types.

Example:

```ts
type UserName = string;
type UserID = number;
```

Now `UserName` represents a string type, and `UserID` represents a number type.

A Type Alias can also define an object structure, just like an interface.

```ts
type User = {
  name: string;
  age: number;
};
```

Both interfaces and Type Aliases can describe object structures, but they have some important differences.

## 7. Type vs Interface

| Feature              | Type Alias       | Interface        |
| -------------------- | ---------------- | ---------------- |
| Keyword              | `type`           | `interface`      |
| Object structure     | Yes              | Yes              |
| Union types          | Yes              | No, not directly |
| Literal types        | Yes              | No, not directly |
| Primitive aliases    | Yes              | No               |
| Extending structures | Intersection `&` | `extends`        |
| Declaration merging  | No               | Yes              |

### When should we use Type?

Use Type Alias when you need:

- Union types.
- Literal types.
- Primitive type aliases.
- Function types.
- Intersections.
- More flexible combinations of types.

### When should we use Interface?

Use Interface when you mainly need:

- Object structures.
- Reusable object definitions.
- Extending other interfaces.
- Declaration merging.

**Important:** Both are useful for object structures. There is no rule that says one is always better than the other.

---

## 8. Extending Interfaces

An interface can inherit properties from another interface using the `extends` keyword.

This allows us to reuse an existing structure instead of writing the same properties again.

```ts
interface Person {
  name: string;
  age: number;
}

interface Student extends Person {
  rollNumber: number;
}
```

The Student interface now contains:

- `name`
- `age`
- `rollNumber`

A Student object must provide all required properties from both interfaces.

### Extending Multiple Interfaces

An interface can extend more than one interface.

```ts
interface Contact {
  email: string;
}

interface Address {
  city: string;
}

interface User extends Contact, Address {
  name: string;
}
```

Now User includes the properties of Contact, Address, and its own `name` property.

**Key point:** `extends` is used with interfaces to build new structures from existing ones.

## 9. Intersection Types (`&`)

Type Aliases can combine multiple types using the intersection operator `&`.

An intersection creates a type that must satisfy all the combined types.

```ts
type Person = {
  name: string;
};

type Contact = {
  email: string;
};

type User = Person & Contact;
```

A User object must contain both `name` and `email`.

### Interface Extension vs Intersection

Both can combine object structures, but they are not identical.

- Interface extension checks compatibility between inherited properties.
- An intersection combines the requirements of both types.
- If the same property has incompatible types, an intersection may produce an impossible property type such as `never`.

Use interface extension when building related interfaces. Use intersections when combining types, especially when working with Type Aliases.

## 10. Declaration Merging

One special feature of interfaces is **Declaration Merging**.

If multiple interfaces have the same name, TypeScript combines their declarations into one interface.

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}
```

TypeScript treats User as an interface containing both `name` and `age`.

However, properties with the same name must have compatible types.

**Important difference:**

- Interfaces with the same name can merge.
- Type Aliases cannot be declared again using the same name in the same scope.

This is one of the major differences between Type and Interface.

---

# Part 3 – Function Types

## 11. Function Parameter Types

In TypeScript, we can specify the type of each function parameter.

This ensures that a function receives the expected type of data.

```ts
function add(a: number, b: number) {
  return a + b;
}
```

Both parameters must be numbers.

If a string is passed instead, TypeScript reports a type error.

Parameter types help prevent incorrect data from entering a function.

## 12. Return Types

We can also specify the type of value a function must return.

```ts
function calculate(a: number, b: number): number {
  return a + b;
}
```

The `: number` after the parameter list specifies the return type.

This means the function must return a number.

TypeScript can often infer the return type automatically, but explicitly defining it can make important function contracts clearer.

**Remember:**

- Parameter types define what a function receives.
- Return types define what a function gives back.

## 13. Void Return Type

The `void` type is used when a function does not return a meaningful value.

```ts
function showMessage(): void {
  console.log("Welcome!");
}
```

This function performs an action but does not return a value.

Common uses include:

- Displaying messages.
- Updating something on the screen.
- Logging information.
- Performing actions without returning a result.

Do not confuse `void` with `undefined` in every situation. `void` describes the intended return behavior of a function.

## 14. Optional Function Parameters

Sometimes a function parameter is not required.

We can make a parameter optional using `?`.

```ts
function greet(name: string, age?: number): void {
  console.log(name);
}
```

Here:

- `name` is required.
- `age` is optional.

An optional parameter may be `undefined` if the caller does not provide it.

**Important rule:** Required parameters should come before optional parameters.

## 15. Default Parameters

A default parameter provides a value automatically when the caller does not supply an argument or passes `undefined`.

```ts
function greet(name: string, city: string = "Karachi"): void {
  console.log(name, city);
}
```

If the city is not provided, the function uses Karachi.

### Optional vs Default Parameters

| Optional Parameter   | Default Parameter                               |
| -------------------- | ----------------------------------------------- |
| Uses `?`             | Uses `=`                                        |
| May be undefined     | Uses a fallback value when omitted or undefined |
| No automatic default | Has a predefined default                        |

Choose optional parameters when the value may genuinely be absent. Choose default parameters when you want the function to use a fallback automatically.

---

## 16. Function Type Alias

We can define the complete structure of a function using a Type Alias.

```ts
type Calculator = (a: number, b: number) => number;
```

This type describes a function that:

- Receives two number parameters.
- Returns a number.

We can use this type when declaring a function:

```ts
const add: Calculator = (a, b) => {
  return a + b;
};
```

TypeScript checks whether the assigned function follows the Calculator structure.

**Important:** The parameter names in the actual function do not have to match the names used in the Type Alias. Their positions and types must match.

Function Type Aliases are useful when multiple functions need to follow the same structure.

## 17. Object Types in Functions

Functions can receive objects as parameters.

We can define the expected object structure using an interface or Type Alias.

For example, a function that receives a Product object should specify which properties that object must contain.

This is useful when:

- Passing user information to a function.
- Calculating product prices.
- Processing student records.
- Working with API data.

Using an interface or Type Alias keeps the function definition cleaner and allows the same object structure to be reused.

---

## 18. Nullish Coalescing (`??`)

Nullish Coalescing is a JavaScript operator that provides a fallback value when the left side is either `null` or `undefined`.

It is particularly useful when working with optional properties and function parameters.

```ts
const discount = undefined;
const finalDiscount = discount ?? 0;
```

Here, the result is `0` because discount is undefined.

### Difference Between `??` and `||`

These operators are not the same.

| Operator | Uses fallback when             |
| -------- | ------------------------------ | --- | -------------- |
| `??`     | Value is `null` or `undefined` |
| `        |                                | `   | Value is falsy |

Falsy values include:

- `0`
- `false`
- `""`
- `null`
- `undefined`
- `NaN`

For example, if a discount is `0`:

- `discount ?? 10` keeps `0`.
- `discount || 10` uses `10`.

**Important:** Use `??` when zero, false, or an empty string are valid values and should not be replaced.

---

# Part 4 – Master Complete Code Example

This example combines the main concepts from Day 3 into one practical program.

It includes:

- Interfaces.
- Optional and readonly properties.
- Nested interfaces.
- Interface extension.
- Type Aliases.
- Intersection types.
- Function parameters and return types.
- Void.
- Default parameters.
- Function Type Alias.
- Nullish Coalescing.

```ts
// 1. Nested Interface
interface Address {
  city: string;
  country: string;
}

// 2. Main Interface
interface Person {
  readonly id: number;
  name: string;
  email?: string;
  address: Address;
}

// 3. Extending Interface
interface Employee extends Person {
  salary: number;
}

// 4. Type Alias
type Status = "active" | "inactive";

// 5. Intersection Type
type EmployeeDetails = Employee & {
  status: Status;
};

// 6. Function Type Alias
type Calculator = (a: number, b: number) => number;

// 7. Function with Parameters and Return Type
function calculateSalary(salary: number, bonus: number = 0): number {
  return salary + bonus;
}

// 8. Function with Void
function showEmployee(employee: EmployeeDetails): void {
  console.log("Employee Information");
  console.log("ID:", employee.id);
  console.log("Name:", employee.name);
  console.log("Email:", employee.email ?? "Not provided");
  console.log("City:", employee.address.city);
  console.log("Country:", employee.address.country);
  console.log("Salary:", employee.salary);
  console.log("Status:", employee.status);
}

// 9. Function using Function Type Alias
const add: Calculator = (a, b) => {
  return a + b;
};

// 10. Employee Object
const employee1: EmployeeDetails = {
  id: 101,
  name: "Ali",
  address: {
    city: "Karachi",
    country: "Pakistan",
  },
  salary: 50000,
  status: "active",
};

// 11. Calling Functions
showEmployee(employee1);

const totalSalary = calculateSalary(employee1.salary, 5000);
console.log("Total Salary:", totalSalary);

console.log("Addition:", add(10, 20));
```

### Understanding the Master Example

**Employee structure:**

The Employee interface extends Person, so it automatically receives the properties defined in Person. It also adds its own salary property.

**Nested Address:**

The address property must follow the Address interface. This keeps address information organized separately.

**Optional Email:**

The email property is optional. If it is not provided, the nullish coalescing operator displays "Not provided".

**Readonly ID:**

The employee ID must be provided when creating the object, but it cannot be reassigned afterward.

**Status Type:**

The Status Type Alias restricts the value to either `"active"` or `"inactive"`.

**Intersection Type:**

EmployeeDetails combines Employee and the status property. Therefore, an employee must satisfy both structures.

**Function Types:**

- `calculateSalary()` accepts salary and an optional bonus through a default value. It returns a number.
- `showEmployee()` receives an EmployeeDetails object and returns no meaningful value.
- `add()` follows the Calculator function type and returns a number.

### Expected Output

```text
Employee Information
ID: 101
Name: Ali
Email: Not provided
City: Karachi
Country: Pakistan
Salary: 50000
Status: active
Total Salary: 55000
Addition: 30
```

---

# Quick Revision

| Concept             | Main Purpose                              |
| ------------------- | ----------------------------------------- |
| Interface           | Defines object structure                  |
| `?`                 | Makes a property or parameter optional    |
| `readonly`          | Prevents reassignment through TypeScript  |
| Nested Interface    | Defines an object inside another object   |
| `Interface[]`       | Defines an array of structured objects    |
| Type Alias          | Creates a reusable name for a type        |
| `extends`           | Extends an interface                      |
| `&`                 | Combines types using intersection         |
| Declaration Merging | Combines same-name interface declarations |
| Parameter Type      | Defines the type of function input        |
| Return Type         | Defines the function's return type        |
| `void`              | Indicates no meaningful return value      |
| Default Parameter   | Provides a fallback parameter value       |
| Function Type Alias | Defines a reusable function structure     |
| `??`                | Provides fallback for null or undefined   |

**Day 3 Key Take away:** Interfaces and Type Aliases help define reusable structures, while function types ensure that functions receive and return the expected data. These concepts make TypeScript code more predictable, organized, and easier to maintain.
