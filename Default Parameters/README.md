# JavaScript Advanced Topics

## Topic #9 — Default Parameters

### Topics Covered

- Default parameter syntax
- `undefined` vs `null`
- Multiple default parameters
- Default expressions
- Parameters depending on other parameters
- Common mistakes and test corrections

---

## 1. What Are Default Parameters?

Default parameters provide a fallback value when an argument is omitted or its value is `undefined`.

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("Ali"); // Hello, Ali!
greet(); // Hello, Guest!
```

**Remember:** The default value is used only when the argument is missing or `undefined`.

## 2. `undefined` vs `null`

| Argument         | Default applies? |
| ---------------- | ---------------- |
| Omitted argument | Yes              |
| `undefined`      | Yes              |
| `null`           | No               |
| `0`              | No               |
| `false`          | No               |
| `""`             | No               |

```javascript
function show(value = 10) {
  console.log(value);
}

show(); // 10
show(undefined); // 10
show(null); // null
show(0); // 0
show(false); // false
show(""); // Empty string
```

**Common mistake:** Thinking that every falsy value triggers a default. Only a missing argument or `undefined` does.

## 3. Multiple Default Parameters

Arguments are assigned according to their positions.

```javascript
function calculate(a = 5, b = 10) {
  console.log(a, b);
}

calculate(); // 5 10
calculate(20); // 20 10
calculate(20, 30); // 20 30
calculate(undefined, 30); // 5 30
```

You can skip the first parameter's value by passing `undefined`, while still providing the second argument.

## 4. Required and Default Parameters

```javascript
function calculateTotal(price, quantity = 1) {
  console.log(price * quantity);
}

calculateTotal(500); // 500
calculateTotal(500, 3); // 1500
```

Here, `price` has no default, but `quantity` defaults to `1`.

If `price` is omitted, it becomes `undefined`. The multiplication then produces `NaN`.

## 5. Default Expressions

A default value can be an expression, not just a fixed number or string.

```javascript
function calculate(a = 10 + 5) {
  console.log(a);
}

calculate(); // 15
calculate(4); // 4
```

The expression is evaluated when the default is needed.

## 6. One Parameter Can Depend on Another

A default parameter can use a parameter declared **before it**.

```javascript
function calculateTax(price, tax = price * 0.1) {
  console.log(tax);
}

calculateTax(1000); // 100
calculateTax(1000, 200); // 200
```

Another example:

```javascript
function test(a = 5, b = a + 2) {
  console.log(a, b);
}

test(); // 5 7
test(10); // 10 12
test(10, 50); // 10 50
```

**Important:** Default parameters are evaluated when needed. A later parameter can use an earlier parameter, but an earlier parameter cannot safely use a later one.

## 7. The Temporal Dead Zone Mistake

```javascript
function test(a = b, b = 5) {
  console.log(a, b);
}

test(); // ReferenceError
```

Why? JavaScript tries to evaluate `a = b` before `b` has been initialized.

**Correct version:**

```javascript
function test(b = 5, a = b) {
  console.log(a, b);
}

test(); // 5 5
```

Put the parameter that supplies the default value first.

## 8. Default Parameters vs. Manual Checks

Before default parameters, developers might write:

```javascript
function greet(name) {
  if (name === undefined) {
    name = "Guest";
  }

  console.log(name);
}
```

With default parameters, the same behavior is simpler:

```javascript
function greet(name = "Guest") {
  console.log(name);
}
```

Use default parameters when you want a fallback for missing or `undefined` arguments.

---

## Quick Revision

1. `function test(x = 10) {}` — calling `test()` gives `10`.
2. `test(undefined)` also uses `10`.
3. `test(null)` gives `null`, not `10`.
4. Arguments are matched to parameters by position.
5. Default parameters can contain expressions.
6. A parameter can depend on an earlier parameter.
7. Referencing a later parameter too early can cause a `ReferenceError`.
8. An explicitly provided argument overrides its default unless that argument is `undefined`.

## Test Correction to Remember

```javascript
function test(a = 3, b = a + 4) {
  console.log(a, b);
}

test(8, undefined); // 8 12
```

The first argument sets `a` to `8`. The second argument is `undefined`, so `b` uses its default expression: `8 + 4 = 12`.

---

**Topic #9 completed:** Default Parameters.

**Next topic in the roadmap:** Topic #10 — First-Class Functions and Higher-Order Functions.
